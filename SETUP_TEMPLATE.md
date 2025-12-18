# download ubuntu cloud 
wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img

# Create Virtual machine
qm create 2222 --name "ubuntu-cloud-template-v2" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0

# Create a virtual disk with the image file
qm importdisk 2222 noble-server-cloudimg-amd64.img local-lvm 

# Attach the disk with image to the created vm
qm set 2222 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-2222-disk-0

# Set the disk as boot disk
qm set 2222 --boot c --bootdisk scsi0

# Resize the vm adding more disk space
qm resize 2222 scsi0 +30G

# Add cloud init drive to the vm
qm set 2222 --ide2 local-lvm:cloudinit

# Enable serial for cloud image
qm set 2222 --serial0 socket --vga serial0

# enable agent
qm set 2222 --agent enabled=1
