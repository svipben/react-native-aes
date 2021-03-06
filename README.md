# React Native AES

AES encryption/decryption for react-native

# DEPRECATED

Use https://github.com/trackforce/react-native-crypto

## Installation
```sh
npm install @trackforce/react-native-aes-crypto
```
or
```sh
yarn add @trackforce/react-native-aes-crypto
```
### Linking Automatically
```sh
react-native link
```
### Linking Manually

#### iOS
* See [Linking Libraries](http://facebook.github.io/react-native/docs/linking-libraries-ios.html)
OR
* Drag RCTAes.xcodeproj to your project on Xcode.
* Click on your main project file (the one that represents the .xcodeproj) select Build Phases and drag libRCTAes.a from the Products folder inside the RCTAes.xcodeproj.

#### (Android)

```gradle
...
include ':@trackforce/react-native-aes-crypto'
project(':@trackforce/react-native-aes-crypto').projectDir = new File(rootProject.projectDir, '../node_modules/@trackforce/react-native-aes-crypto/android')
```

* In `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':@trackforce/react-native-aes-crypto')
}
```

* register module (in MainApplication.java)

```java
......
import com.trackforce.aes.RCTAesPackage;

......

@Override
protected List<ReactPackage> getPackages() {
   ......
   new RCTAesPackage(),
   ......
}
```

## Usage

### Example

```js
import AES from '@trackforce/react-native-aes-crypto';

function generateKey(password, salt) {
    return AES.pbkdf2(password, salt);
}

async function encrypt(text, key) {
    const iv = 'base 64 random 16 bytes string';
    try {
        const ciphertext = await AES.encrypt(text, key, iv);
        return { ciphertext, iv };
    } catch (error) {
        throw error;
    }
}

function decrypt(ciphertext, key, iv) {
    return AES.decrypt(ciphertext, key, iv);
}

function hmac(ciphertext, key) {
    return AES.hmac256(ciphertext, key);
}

(async () => {
    try {
        const generatedKey = await generateKey('password', 'salt');
        console.log(`generatedKey: ${generatedKey}`);

        const { ciphertext, iv } = await encrypt('Hello, world!', generatedKey);
        console.log(`ciphertext: ${ciphertext}, iv: ${iv}`);

        const decryptedText = await decrypt(ciphertext, generatedKey, iv);
        console.log(`decrypted: ${decryptedText}`);

        const hash = await hmac(ciphertext, generatedKey);
        console.log(`hash: ${hash}`);
    } catch (error) {
        throw error;
    }
})();
```

### methods

- `encrypt(text, key, iv)`
- `decrypt(base64, key, iv)`
- `pbkdf2(text, salt)`
- `hmac256(cipher, key)`
- `sha1(text)`
- `sha256(text)`
- `sha512(text)`
