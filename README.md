# 📥 ChicKernel permalinks
### [**GitHub Releases**](https://github.com/chickendrop89/device_xiaomi_unified-kernel/releases/latest) ◭ [**SourceForge mirror**](https://sourceforge.net/projects/chickernel-xiaomi-tapas-kernel) ◮ [**XDA Forums thread**](https://xdaforums.com/t/kernel-chickernel-a-kernel-optimized-for-smoothness-and-low-memory.4678538) 

# 📝 Note:
This kernel is tuned for these SDM685 (`sm6225-AD`) devices with `4/6GB` RAM:
- Xiaomi Redmi Note _12_ 4G (`topaz` / `tapas`)
- Xiaomi Redmi Note _13_ 4G (`sapphire` / `sapphiren`)

# ⛔ Report Issues
Please [report issues here](https://github.com/chickendrop89/device_xiaomi_unified-recovery/issues), or to [my telegram](https://t.me/chickendrop89)

# 🏗️ Build notes:
### **Cloning this source and build environment**:
- There is a [custom `repo` manifest](https://github.com/chickendrop89/device_xiaomi_unified-kernel/blob/readme/chickernel.xml) in this branch root directory, Download it, and move it into some directory.
- After that, (expecting the manifest is in the current directory) execute these commands
```shell
repo init -u https://android.googlesource.com/kernel/manifest -m $(pwd)/chickernel.xml
repo sync --fetch-submodules
```

### **Building this source**:
- https://source.android.com/docs/setup/build/building-kernels#building
- You can either use `bazel` or `build.sh (deprecated)` to build a GKI kernel. I personally hate bazel.

```shell
BUILD_CONFIG=common/<build config>
FAST_BUILD=1 # Not required, forces thinLTO

build/config.sh nconfig # Edit build configuration
build/build.sh # Build the kernel
```
- After building, the artifacts can be found at `out/dist`

### **This kernel is built with these configurations:**
- `build.config.gki.aarch64.chickernel`
- `build.config.gki.aarch64.chickernel.ksun`
- `build.config.gki.aarch64.chickernel.ksun.susfs`

### **Building using CI/CD (Actions)**
- GitHub actions can be used to automatically build the kernel with all of it's configurations
- It requires a custom `manifest.xml` (with a change to the `common-repo` to your fork's URL) and other stuff
- https://github.com/chickendrop89/gki-build-workflow

### **Upstreaming the source:**
- Example: upstreaming the kernel to latest ACK LTS
```shell
git remote add ack https://android.googlesource.com/kernel/common
git fetch ack android13-5.15-lts
git merge android13-5.15-lts

# Then fix merge conflicts if any
```

### **Upstreaming KernelSU Next:**
- This is dependent on how you cloned this repository!
- Example: change directory to your source, and run:
```shell
# If managed by 'repo'
repo sync --fetch-submodules

# If managed by 'git'
git submodule update --init --remote
```

### **Upstreaming SuSFS:**
- Upstreaming SuSFS implementation in kernel is kinda tricky, because it involves applying patches, and re-doing everything
- Clone the [`gki-android13-5.15`](https://gitlab.com/simonpunk/susfs4ksu/-/tree/gki-android13-5.15) branch into your working directory outside the kernel, 
and after you do the 1st step mentioned below, do everything as according the guide in it's [README](https://gitlab.com/simonpunk/susfs4ksu/-/blob/gki-android13-5.15/README.md)

I do it someway like this:
1. First, [i revert previous implementation commit](https://github.com/chickendrop89/device_xiaomi_unified-kernel/commit/07f7d604fb4695e0735f0ab3e88e6ed57a90adf3)
2. I merge the directories from `kernel_patches/` to my tree
3. I apply the patch: `git apply *.patch --reject --whitespace=fix`

### **A note about the deleted SuSFS branch**
- SuSFS was merged into the main branch for the sake of simplicity and my sanity
