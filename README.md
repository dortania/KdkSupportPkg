# KdkSupportPkg

Repository dedicated to Kernel Debug Kit archival, with primary focus on macOS Ventura KDKs in relation to Root Volume Patching with OpenCore Legacy Patcher.

----------

With macOS 13, Ventura, Apple dropped on-disk kernel binaries in `/System/Library/Extensions`. Due to this, end users cannot build Boot and System Kernel Collections without manually install a Kernel Debug Kit from Apple's Developer Site. However due to Apple's account requirement for downloads, automated retrival is not possible. Thus this repo will create a release for each KDK seeded, with the tag representing the build associated.

Source for Kernel Debug Kits:

* [Apple Developer Site: More Downloads](https://developer.apple.com/download/all/)

----------

Example of pulling releases:

```py
#!/usr/bin/env python3

REQUESTED_BUILD = "22A5365d"

KDK_MIRROR_REPOSITORY = "https://api.github.com/repos/dortania/KdkSupportPkg/releases"

catalog = requests.get(KDK_MIRROR_REPOSITORY)
if catalog.status_code != 200:
    # Can't reach Github
    return None

catalog = catalog.json()

for release in catalog:
    if release["tag_name"] == REQUESTED_BUILD:
        for asset in release["assets"]:
            if asset["name"].endswith(".dmg"):
                # Returns URL to rehosted Kernel Debug Kit
                return asset["browser_download_url"]

return None
 ```
 
