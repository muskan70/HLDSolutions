## Design Google Drive System

### Functional Requirements
1. Add and download files.
2. Sync files across multiple devices. When a file is added to one device, it is automatically synced to other devices.
3. See file revisions i.e older versions.
4. Share files with other users.
5. Send a notification when a file is edited, deleted, or shared with you.

### Non Functional Requirements
1. **Reliability**: Reliability is extremely important for a storage system. Data loss is unacceptable.
2. **Fast sync speed**: If file sync takes too much time, users will become impatient and abandon the product.
3. **Scalability**: The system should be able to handle high volumes of traffic.
4. **High availability**: Users should still be able to use the system when some servers ar offline, slowed down, or have unexpected network errors.

### Back of the envelope estimation
- Assume the application has 50 million signed up users and 10 million DAU.
- Users get 10 GB free space.
- Assume users upload 2 files per day. The average file size is 500 KB.
- 1:1 read to write ratio.
- Total space allocated: 50 million * 10 GB = 500 Petabyte
- QPS for upload API: 10 million * 2 uploads / 24 hours / 3600 seconds = ~ 240
- Peak QPS = QPS * 2 = 480