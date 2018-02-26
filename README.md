# Expo-image-upload-with-php

# In Expo App
```
async function takePhotoAndUpload() {

  let result = await ImagePicker.launchCameraAsync({
    allowsEditing: false, // higher res on iOS
    aspect: [4, 3],
  });

  if (result.cancelled) {
    return;
  }

  let localUri = result.uri;
  let filename = localUri.split('/').pop();

  let match = /\.(\w+)$/.exec(filename);
  let type = match ? `image/${match[1]}` : `image`;

  let formData = new FormData();
  formData.append('photo', { uri: localUri, name: filename, type });

  return await fetch('http://example.com/upload.php', {
    method: 'POST',
    body: formData,
    header: {
      'content-type': 'multipart/form-data',
    },
  });
}
```

# In PHP Server upload.php
```
<?php
    move_uploaded_file($_FILES['photo']['tmp_name'], './photos/' . $_FILES['photo']['name']);
?>
```
