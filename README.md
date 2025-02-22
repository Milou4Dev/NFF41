## Prerequisites

1. **Updated System**  
   Ensure all packages are current:
   ```
   sudo dnf update --refresh && sudo reboot
   ```

2. **RPM Fusion Repositories**  
   Enable third-party repositories:
   ```
   sudo dnf install \
   https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-41.noarch.rpm \
   https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-41.noarch.rpm
   ```

---

## Installation Steps

### 1. Install NVIDIA Drivers
```
sudo dnf install akmod-nvidia
```

### 2. Install Optional Components
- **CUDA Support**:
  ```
  sudo dnf install xorg-x11-drv-nvidia-cuda
  ```
- **32-bit Libraries** (for Steam/Proton):
  ```
  sudo dnf install xorg-x11-drv-nvidia-libs.i686
  ```

### 3. Rebuild Kernel Modules
```
sudo akmods --force && sudo dracut --force
```

---

## Post-Installation Configuration

### 1. Enable Kernel Mode Setting (KMS)
```
sudo grubby --update-kernel=ALL --args='nvidia-drm.modeset=1'
```

### 2. Configure Initramfs
Create `/etc/dracut.conf.d/nvidia.conf` with:
```
add_drivers+=" nvidia nvidia_modeset nvidia_uvm nvidia_drm "
install_items+=" /etc/modprobe.d/nvidia.conf "
```

Regenerate initramfs:
```
sudo dracut --force
```

### 3. Enable Wayland (Optional)
Edit `/etc/gdm/custom.conf` and remove `WaylandEnable=false`.  

---

## Verification

1. **Check Driver Version**:
   ```
   nvidia-smi
   ```

2. **OpenGL Rendering**:
   ```
   glxinfo | grep "OpenGL renderer"
   ```

3. **Vulkan Support**:
   ```
   vulkaninfo | grep "GPU id"
   ```
