### How to utilize Google Drive for saving Bandwidth


[Google Drive API - Change “Shared” setting](http://stackoverflow.com/questions/19958821/google-drive-api-change-shared-setting) shows a sample code to change files on Google Drive to be shared publicly:

```javascript
var resource = {
  'value': 'default',
  'type': 'anyone',
  'role': 'reader'
};
var request = gapi.client.drive.permissions.insert({
  'fileId': fileId,
  'resource': resource
}).execute(function() { 
  alert("file set to public")
});
```

And, use `downloadUrl` specified in a [file](https://developers.google.com/drive/v2/reference/files) for client app to download the file.


## How to upload generated APKs onto Google Drive

Using [Files: insert](https://developers.google.com/drive/v2/reference/files/insert) to upload the generated APK file. Size limitation is 10GB (enough for seadragon idea!!)

(Codes running in browser, needs to be changed when using nodejs)

```javascript
/**
 * Insert new file.
 *
 * @param {File} fileData File object to read data from.
 * @param {Function} callback Function to call when the request is complete.
 */
function insertFile(fileData, callback) {
  const boundary = '-------314159265358979323846';
  const delimiter = "\r\n--" + boundary + "\r\n";
  const close_delim = "\r\n--" + boundary + "--";

  var reader = new FileReader();
  reader.readAsBinaryString(fileData);
  reader.onload = function(e) {
    var contentType = fileData.type || 'application/octet-stream';
    var metadata = {
      'title': fileData.fileName,
      'mimeType': contentType
    };

    var base64Data = btoa(reader.result);
    var multipartRequestBody =
        delimiter +
        'Content-Type: application/json\r\n\r\n' +
        JSON.stringify(metadata) +
        delimiter +
        'Content-Type: ' + contentType + '\r\n' +
        'Content-Transfer-Encoding: base64\r\n' +
        '\r\n' +
        base64Data +
        close_delim;

    var request = gapi.client.request({
        'path': '/upload/drive/v2/files',
        'method': 'POST',
        'params': {'uploadType': 'multipart'},
        'headers': {
          'Content-Type': 'multipart/mixed; boundary="' + boundary + '"'
        },
        'body': multipartRequestBody});
    if (!callback) {
      callback = function(file) {
        console.log(file)
      };
    }
    request.execute(callback);
  }
}
```
