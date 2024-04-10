# uploading files to storage
we must create a reference to the specific path of the file within our bucket
- we use the ref function to do this
    - returns a storage reference to the location in the bucket that corresponds to the provided URL
    - takes the storage instance as first parameter
    - takes OPTIONAL second parameter of a URL to the file to which the reference will point
        - if no URL is provided, returns a reference to the root of the storage bucket

```
import { getStorage, ref } from "firebase/storage";
import { firebaseApp } from "../services/firebase";

const storage = getStorage(firebaseApp);

// reference to root of Cloud Storage bucket
const storageRef = ref(storage)

// reference to logo.png
const logoRef = ref(storage, "logo.png");

// reference to educative directory
const educativeRef = ref(storage, "educative");

// reference to educative/logo.png
const educativeLogoRef = ref(storage, "educative/logo.png");
```

## simple file upload
**firebase storage SDK supports the following file formats:**
- JavaScript file
- Blob APIs
- Uint8Array Byte Arrays
- raw
- base64
- base64url
- data_url encoded strings

to upload a JavaScript File and Blob APIs, we can use the uploadBytes function
- takes a reference to where the data should be uploaded as first parameter
- takes the data to upload as second parameter
- OPTIONAL third parameter represents the metadata properties such as name, contentType, and size of the file to be uploaded

```
import { getStorage, ref, uploadBytes } from "firebase/storage";
import { firebaseApp } from "../services/firebase";

const storage = getStorage(firebaseApp);

// reference to educative directory
const educativeRef = ref(storage, "images/logo.png");

export const fileUpload = (image) => {
  const metadata = {
    contentType: "image/png",
  };

  uploadBytes(educativeRef, image, metadata)
    .then((snapshot) => {
      alert("File uploaded successfully");
    })
    .catch((err) => {
      console.log(err);
    });
};
```

***files uploaded with the uploadBytes function are not resumable**
- cannot be paused, resumed, or cancelled