# Codesigning for Apple Developers

With the introduction of Apple's EndpointSecurity framework ([PR
6467](https://github.com/osquery/osquery/pull/6467), we've started to
need entitlements, profiles, and codesigning. Even in development.

As I understand Apple's model:
1. apps/packages/etc are signed by certs. 
2. There are _many_ different flavors of certificate. For our
   purposes, the ones that matter:
   - Developer ID Installer: Privileged. Allows installer distribution
     outside the app store.
   - Developer ID Application: Privileged. Allows app distribution
     outside the app store.
   - Apple Development / Mac Development: Developer. Allows signing
     development versions.
3. The privileged ones, coupled with an entitlements file allow
   osquery to work.
4. The developer ones _also_ require a profile, which only works on
   macs identified by uuid.
   - `select uuid from system_info`
5. Apple accounts can have members that are `developers`. These
   members can create personal developer certificates, but cannot
   create distribution ones. Cannot adjust devices. Cannot adjust
   profiles. According, it should be pretty reasonable to add osquery
   developers to the account.
   
## Process for adding a new developer

_*Note*: This is for adding a new developer to the account. This
should not grant distribution permission, but they should still be somewhat trusted._

Create a foundation ticket for general tracking and details. If any of
this is considered private, consider using slack to communicate
details.

We will need the email address for their apple account. They can
create a new one, or use an existing one. This should be a
human. Apple does not like roll accounts.

We need their test machine[s] uuid. This can be gathered with `select
uuid from system_info;`

### Admin Actions

Login to https://developer.apple.com/

Under `Certificates, Identifiers & Profiles` find `Devices`. Add their
uuid. Name the device something reasonable.

Find the people tab, it will take you to App Store Connect

Add them as a `Developer` Check the box to allow access to
certificates.

### Developer Actions

Xcode might be able to do this for you. These are manual instructions. 

Use `Certificate Assistant`:
1. Open `Keychain Access`
2. `Keychain Access` menu -> `Certificate Assistent` -> `Request a Certificate from a CA`
3. Use your apple id email address (might not matter)
4. Maybe sure it's saved to disk
5. Save the CSR somewhere

Login to https://developer.apple.com/

You should have access to the osquery account, `3522FA9PXF`. If you
have multiple accounts, it will be in the top right pulldown.

Click `Certificates, Identifiers & Profiles` and create a certificate. 

You'll want either an `Apple Development` or a `Mac Development`
certificate.

Upload the CSR generated earlier. 

You should now be able to download the certificate. 

### Back to the Admin

Now the admin will have to add the developer and the device to the
profile. Or create one specific to this developer. Probably doesn't
matter much.

### Back to the developer

Finally, you can download a profile. This needs to be installed on the
machines to allow the machine to trust the code signing.

## How to Use Endpoint Security

_*NOTE*: you need to be building on catalina or later_

Build something. This is using some demo esf code:

Cannot run without any signatures:

``` shell
$ ./endpointsecurity
client lacks entitlement
```


Code sign without entitlements

``` shell
$ codesign --force -s "${CODESIGN_IDENTITY}" -v --options runtime,library --timestamp ./endpointsecurity

$ ./endpointsecurity
client lacks entitlement
```

Finally, with entitlements. 

``` shell
$ codesign --force -s "${CODESIGN_IDENTITY}" -v --options runtime,library --timestamp --entitlements endpointsecurity.entitlements ./endpointsecurity

$ ./endpointsecurity.entitled 
Killed: 9
```


Oh no! We still need to install the profile authorizing this host. 

Still `killed: 9`
