23-December-2025


In Azure, 

- a **Compute Gallery** is just a "Folder"
- and the **Image Definition** is the "Template" inside it.

### 📝 Simplified Notes: Azure Compute Gallery

```hcl
# 1. THE GALLERY (The "Folder" to store images)
resource "azurerm_shared_image_gallery" "main" {
  name                = "my_image_gallery"
  resource_group_name = "my-resources"
  location            = "East US"
  description         = "Storage for my AVD images"
}

# 2. THE IMAGE DEFINITION (The "Blueprint" or Template)
resource "azurerm_shared_image" "win11_avd" {
  name                = "Win11-AVD-Template"
  gallery_name        = azurerm_shared_image_gallery.main.name
  resource_group_name = "my-resources"
  location            = "East US"
  
  os_type             = "Windows"
  hyper_v_generation  = "V2"

  identifier {
    publisher = "MyCompany"
    offer     = "Windows11"
    sku       = "avd-standard"
  }
}

```

---

### 💡 Key Concepts for Beginners

- **Gallery vs. Image:** The `gallery` is the container. The `shared_image` is the definition (metadata) of the VM you want to build.
- **Hyper-V Generation:** Always use **V2** for modern Azure VMs (like Windows 11).
- **Packer Note:** You don't need a `shared_image_version` block in Terraform if you use **Packer**. Packer will build the actual disk and "plug it into" this definition automatically.

---

### 🚀 Summary Table for your Article

| Resource | Purpose | Real-world Analogy |
| --- | --- | --- |
| **Shared Image Gallery** | The Storage System | A Bookshelf |
| **Shared Image** | The Blueprint | A Book Title |
| **Image Version** | The actual Build (Packer) | The Edition/Page of the book |

**Would you like me to explain how Packer connects to this Terraform code for your next article step?**
