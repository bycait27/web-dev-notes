# download files from storage
we need to create a reference to the file to download files from the cloud storage bucket

## simple file download
we can use the getDownloadURL function to download an existing file from the storage bucket
- need the reference that holds the file in the bucket as a parameter
- RETURNS A JAVASCRIPT PROMISE 
    - RESOLVES WITH THE DOWNLOAD URL OF THE FILE

***the file below can be downloaded as a blob or inserted directly into the HTML with an img tag**

```
import { getStorage, ref, getDownloadURL } from "firebase/storage";
import { firebaseApp } from "../services/firebase";

const storage = getStorage(firebaseApp);

// reference to educative directory
const educativeRef = ref(storage, "images/logo.png");

export const fileDownload = () => {
  getDownloadURL(educativeRef)
    .then((url) => {
      console.log(url);
    })
    .catch((err) => {
      console.log(err);
    });
};
```