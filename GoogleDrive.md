## Google Drive System Design

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

### API Design
1. **Upload a file to Google Drive**: Two types of uploads are supported
- Simple upload: Use this upload type when the file size is small.
- Resumable upload: Use this upload type when the file size is large and there is high chance of network interruption. A resumable upload is achieved by the following steps : Send the initial request to retrieve the resumable URL. Upload the data and monitor upload state. If upload is disturbed, resume the upload.
- POST /api/v1/files/upload?uploadType=resumable
- RequestBody : { data: Local file to be uploaded }

2. **Download a file from Google Drive**
- GET /api/v1/files/download
- RequestBody: { path: download file path }

3. **Get file revisions**
- POST /api/v1/files/list_revisions
- RequestParams: { path: The path to the file you want to get the revision history, limit: The maximum number of revisions to return }

### File Upload Flow
Instead of uploading a file as one unit, split file into smaller chunks.
- Pros: parallel uploading to S3, only load modified chunks on file changes
- Cons: Added complexity on client's end to do file diff, also have to keep track of file chunks in DB.

### Database Design
1. UserDB
- Schema -> { userId, name, email, passwordHash ...}
- MySQL: shard + index on userId

2. FilePermissionsDB
- Schema -> { userId, docId}
- MySQL: shard + index on userId

3. FileDetailsDB
- Schema -> {fileId, timestamp, fileName, description}
- MySQL: Shard by fileId

5. FileChunkMetadataDB
- Schema -> {fileId, version, chunkHash, URL}
- MySQL: partition on fileId to get all chunks of file on same node
- Indexing on fileId + version

6. FileChunk Storage
- A blob storage system is used to store chunks of files
- S3: less expensive, option2 can be HDFS: offers better data locality but expensive


