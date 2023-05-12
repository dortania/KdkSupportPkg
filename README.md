# KdkSupportPkg

Repository dedicated to Kernel Debug Kit archival, with a primary focus on macOS Ventura KDKs in relation to Root Volume Patching with OpenCore Legacy Patcher.

----------

With macOS 13, Ventura, Apple dropped on-disk kernel binaries in `/System/Library/Extensions`. Due to this, end users cannot build Boot and System Kernel Collections without manually installing a Kernel Debug Kit from Apple's Developer Site. However, due to Apple's account requirement for downloads, automated retrieval is not possible. Thus this repo will create a release for each KDK seeded, with the tag representing the build associated.

Source for Kernel Debug Kits:

* [Apple Developer Site: More Downloads](https://developer.apple.com/download/all/)

----------

Example of pulling releases:

```py
#!/usr/bin/env python3

KDK_API_LINK:        str = "https://raw.githubusercontent.com/dortania/KdkSupportPkg/gh-pages/manifest.json"
KDK_REQUESTED_BUILD: str = "22F5059b"

catalog = requests.get(KDK_API_LINK)
if catalog.status_code != 200:
    # Can't reach Github
    return None

catalog = catalog.json()

for kdk in catalog:
    if (kdk["build"] != KDK_REQUESTED_BUILD):
        continue

    return {
        "url":      kdk["url"],      # str (DMG URL),  ex: https://.../Kernel_Debug_Kit_13.4_build_22F5059b.dmg
        "build":    kdk["build"],    # str (22xxxxxx), ex: 22F5059b
        "version":  kdk["version"],  # str (x.y.z),    ex: 13.4
        "fileSize": kdk["fileSize"]  # int (bytes),    ex: 654356659
    }

return None
 ```

For a more in-depth implementation, see OpenCore Legacy Patcher's [kdk_handler.py's `KernelDebugKitObject` class](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/a6e0c142ca8c4aacf1741eeeb58215a037578f91/resources/kdk_handler.py).
