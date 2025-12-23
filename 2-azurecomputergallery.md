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


To understand how **Packer** and **Terraform** work together, you have to think of it as a **"Two-Step Dance."** Terraform builds the **Shelf** (Gallery), and Packer creates the **Book** (Image) to put on it.

---

### 🛠️ The Workflow: How Packer connects to Terraform

When you use Packer, you don't need Terraform to manage the specific "versions" of your images. Here is the simple flow:

1. **Terraform's Job (The Infrastructure):** You run Terraform first. It creates the **Resource Group**, the **Compute Gallery**, and the **Image Definition** (the empty blueprint we simplified above).
2. **Packer's Job (The Software):** Packer starts a temporary VM, installs Windows 11, adds your development tools (VS Code, Chrome, etc.), runs **Sysprep**, and then "Captures" that VM.
3. **The Handshake:** In your Packer code, you tell it the name of the Gallery and Image Definition you created with Terraform. Packer then creates a **New Version** (e.g., `1.0.0`) inside that gallery.

---

### 📝 Simplified Packer Snippet (The Connection)

In your Packer template (`.pkr.hcl`), this is how you point to the Terraform resources:

```hcl
# This tells Packer WHERE to put the finished image
shared_image_gallery_destination {
    subscription         = "your-sub-id"
    resource_group       = "my-resources"     # Created by Terraform
    gallery_name         = "my_image_gallery" # Created by Terraform
    image_name           = "Win11-AVD-Template" # Created by Terraform
    image_version        = "1.0.0"            # Packer creates this
    replication_regions  = ["East US"]
}

```

---

### 💡 Why is this better for AVD?

* **Speed:** You don't have to manually install software every time you make a VM.
* **Consistency:** Every developer gets the exact same Windows 11 setup.
* **Automation:** When you update your code in GitLab, Packer builds a new version, and your AVD Host Pool can automatically start using the new "Gold Image."

### 🚀 Next Step for your Lab:

Since you are writing an article, would you like a **"Simple Comparison Table"** between **Terraform** and **Packer** so your readers know which tool to use for what?
