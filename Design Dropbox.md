# Design DropBox
## Clarify
### Requirement
1. Users should be able to upload and download their files/photos from any device.
2. Our service should support automatic synchronization between devices, i.e., after updating a file on one device, it should get synchronized on all devices.


### Design Considerations
1. save file in chunks (2mb - 4mb)
	- If a user fails to upload a file, then only the failing chunk will be retried.
	- if change the file on local, only need to update small chunk of file in storage, instead have another copy of the whole file
	- Allow multi-thread work on upload/download different part of chunk.

2. Use metadata file to document all the chunks information
	

## Service
### Overview
