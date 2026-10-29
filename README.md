# Bigby.office

Bigby.office is the office suite in Bigby.cloud's Office and Group plans. It lets you open and edit documents, spreadsheets and presentations in your browser, and work on them with other people at the same time.

It's built on [Collabora Online](https://www.collaboraonline.com/), the open source office suite developed by Collabora Productivity. This repository is their code with our branding and fixes on top. [MAINTAINING.md](MAINTAINING.md) explains how we keep it up to date with their releases.

## Reporting a problem

* For problems with Bigby.office or Bigby.cloud, use our [report form](https://bigby.cloud/report-an-issue/).
* If the bug is in Collabora Online itself, you can also report it to Collabora on their [issue tracker](https://github.com/CollaboraOnline/online/issues/new/choose).

See [CONTRIBUTING.md](CONTRIBUTING.md) if you'd like to send a change.

## Licence

Most of the code is under the [MPLv2](http://mozilla.org/MPL/2.0/) licence. Some parts are under other open source licences, see for example [browser/LICENSE](browser/LICENSE).

## Developers

### Development bits

This project has several components:
* **engine/**
  * The document rendering engine
* **wsd/**
  * The Web Services Daemon - which accepts external connections
* **kit/**
  * The client which lives in its own chroot and renders documents
* **common/**
  * Shared code between these processes
* **browser/**
  * The client side JavaScript component
* **test/**
  * C++ based unit tests
* **cypress_test/**
  * JavaScript based integration tests

### Building for development

The engine (core) is built first, then online on top of it.

    cd engine
    ./autogen.sh --with-distro=CPLinux-LOKit-Dev
    make
    cd ..
    ./autogen.sh --enable-developer
    make

Then "make run" to start coolwsd. Both developer options are convenience
bundles; pass individual configure options to override a piece. To bootstrap
from scratch, see scripts/clone-online.sh.

See also README-gbuild.md for an ongoing merge of the two-step engine and online builds.

### Software Bill of Materials

The build generates SBOMs for the server, the browser client, the
engine and the container images. [SBOM.md](SBOM.md) describes how they are
generated, how to check them (`make sbom-check`, `make sbom-audit`,
`make sbom-freshness`), how to scan them for known vulnerabilities, and what
a clean scan does *not* tell you.

### Further reading

Collabora's guide to [building Collabora Online](https://www.collaboraoffice.org/post/build-code/) covers the build in more detail.

### Testing with Nextcloud or ownCloud

Set up Nextcloud or ownCloud on localhost and install the richdocuments
app. Good tutorials exist for installing ownCloud or Nextcloud, so we don't
repeat them here. In the app stores the richdocuments app is listed as
"Collabora Online".

When you have a running Nextcloud or ownCloud instance at
http://localhost/nextcloud or at http://localhost/owncloud
go to the "Collabora Online" settings page, and set the WOPI URL to
http://localhost:9980

Then in the build tree, edit the generated coolwsd.xml and set ssl setting to
false. You can run make run, and test coolwsd with the ownCloud or Nextcloud
integration.

Note: if SSL is enabled in either Online or the integration, both must
have SSL enabled. That is, you must access NC/OC using https:// as well
as set the WOPI URL in NC/OC to https://localhost:9980.

### Admin Panel

You can access the admin panel by directly accessing the admin.html file
from browser directory.

Websocket connections to admin console can be made at path: /adminws/ on the
same url and port as coolwsd is running on. However, one needs a JWT token to
authenticate to the admin console websocket. This is stored as a cookie with
`Path: /adminws/` when user successfully authenticates when trying to access
/browser/dist/admin/admin\*html files (HTTP Basic authentication). Token
is expired after every half an hour, so websocket connection to admin console
must be established within this period.

It should also be possible to do various sorts of tasks such as killing
documents that are open for more than 10 hours etc. See protocol.txt for
various commands. Only tricky thing here is getting the JWT token which can
be obtained as described above.

### Protocol description

See **[protocol.txt](wsd/protocol.txt)** for a description of the protocol to be used over the
websocket.

### Architecture

Collabora's SDK documentation describes the [architecture](https://sdk.collaboraonline.com/docs/architecture.html).
