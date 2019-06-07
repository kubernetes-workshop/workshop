# Azure Volumes

There are two main types of Azure Volumes:
* azureFile
* azureDisk

## important terms
* PersistentVolume
* PersistentVolumeClaim
* StorageClass
* TODO

TODO: Grafik mit allen Begriffen als Erklärung


## Background Information

Azure Documentation in general
* [Azure Storage Introduction](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction)
* [Azure Storage differences: Blobs, Files, Disks](https://docs.microsoft.com/en-us/azure/storage/common/storage-decide-blobs-files-disks)

Azure Documentation about Kubernetes Storages
* [storage concepts](https://docs.microsoft.com/en-us/azure/aks/concepts-storage)

deep dive:
* [Azure Managed Disks: lessons learned](https://blogs.msdn.microsoft.com/igorpag/2017/03/14/azure-managed-disks-deep-dive-lessons-learned-and-benefits)
* [Storage: AWS vs Azure](https://cloud.netapp.com/blog/aws-vs-azure-cloud-storage-comparison)

code samples:
* [official kubernetes examples](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_file)