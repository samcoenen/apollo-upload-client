![Apollo upload logo](https://cdn.rawgit.com/jaydenseric/apollo-upload-client/v7.0.0-alpha.3/apollo-upload-logo.svg)

# apollo-upload-client

[![npm version](https://img.shields.io/npm/v/apollo-upload-client.svg)](https://npm.im/apollo-upload-client) ![Licence](https://img.shields.io/npm/l/apollo-upload-client.svg) [![Github issues](https://img.shields.io/github/issues/jaydenseric/apollo-upload-client.svg)](https://github.com/jaydenseric/apollo-upload-client/issues) [![Github stars](https://img.shields.io/github/stars/jaydenseric/apollo-upload-client.svg)](https://github.com/jaydenseric/apollo-upload-client/stargazers)

Enhances [Apollo](https://apollographql.com) for intuitive file uploads via GraphQL queries or mutations. Use with [apollo-upload-server](https://github.com/jaydenseric/apollo-upload-server) or [apollo_upload_server-ruby](https://github.com/jetruby/apollo_upload_server-ruby).

## Setup

Install with peer dependencies using [npm](https://npmjs.com):

```shell
npm install apollo-upload-client apollo-link graphql
```

Initialize Apollo Client with this terminating link:

```js
import { createUploadLink } from 'apollo-upload-client'

const link = createUploadLink(/* Options */)
```

See also the [setup instructions](https://github.com/jaydenseric/apollo-upload-server#setup) for the [`apollo-upload-server`](https://github.com/jaydenseric/apollo-upload-server) middleware.

### Options

`createUploadLink` options match [`createHttpLink` options](https://www.apollographql.com/docs/link/links/http.html#Options):

* `includeExtensions` (boolean): Toggles sending `extensions` fields to the GraphQL server. (default: `false`).
* `uri` (string): GraphQL endpoint URI (default: `/graphql`).
* `credentials` (string): Overrides `fetchOptions.credentials`.
* `headers` (object): Merges with and overrides `fetchOptions.headers`.
* `fetchOptions` (object): [`fetch` init](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters); overridden by upload requirements.
* `fetch` (function): [Fetch API](https://fetch.spec.whatwg.org) to use (default: Global `fetch`).

## Usage

Use [`File`](https://developer.mozilla.org/en/docs/Web/API/File), [`FileList`](https://developer.mozilla.org/en/docs/Web/API/FileList) or [`ReactNativeFile`](#react-native) instances anywhere within query or mutation input variables. For server instructions see [`apollo-upload-server`](https://github.com/jaydenseric/apollo-upload-server). See the [example API and client](https://github.com/jaydenseric/apollo-upload-examples).

### [`File`](https://developer.mozilla.org/en/docs/Web/API/File) example

```jsx
import gql from 'graphql-tag'
import { graphql } from 'react-apollo'

export default graphql(gql`
  mutation($file: Upload!) {
    uploadFile(file: $file) {
      id
    }
  }
`)(({ mutate }) => (
  <input
    type="file"
    required
    onChange={({ target }) =>
      target.validity.valid && mutate({ variables: { file: target.files[0] } })
    }
  />
))
```

### [`FileList`](https://developer.mozilla.org/en/docs/Web/API/FileList) example

```jsx
import gql from 'graphql-tag'
import { graphql } from 'react-apollo'

export default graphql(gql`
  mutation($files: [Upload!]!) {
    uploadFiles(files: $files) {
      id
    }
  }
`)(({ mutate }) => (
  <input
    type="file"
    multiple
    required
    onChange={({ target }) =>
      target.validity.valid && mutate({ variables: { files: target.files } })
    }
  />
))
```

### React Native

Substitute [`File`](https://developer.mozilla.org/en/docs/Web/API/File) with `ReactNativeFile` from [`extract-files`](https://github.com/jaydenseric/extract-files):

```js
import { ReactNativeFile } from 'apollo-upload-client'

const file = new ReactNativeFile({
  uri: '…',
  type: 'image/jpeg',
  name: 'photo.jpg'
})

const files = ReactNativeFile.list([
  {
    uri: '…',
    type: 'image/jpeg',
    name: 'photo-1.jpg'
  },
  {
    uri: '…',
    type: 'image/jpeg',
    name: 'photo-2.jpg'
  }
])
```

## Support

* Node.js v6.10+, see `package.json` `engines`.
* [Browsers >1% usage](http://browserl.ist/?q=%3E1%25), see `package.json` `browserslist`.
* React Native.
