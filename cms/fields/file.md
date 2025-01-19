---
title: File
description: Field to upload and store files
---

The field of type `file` is used to upload and store files. It display a
`<input type="file">` in the UI.

## Example

```ts
{
  name: "image",
  type: "file",
  uploads: "uploads",
}
```

## Available options

In addition to the common options (see
[fields documentation](../configuration/fields.md#common-field-options) for more
info), this field has the following options:

### upload

The name of the [upload entity](../configuration/uploads.md) used to store the
file. It's a string with the format `upload_name:directory`. For example, if you
have an Upload registered with the name `images` and want to save the files in
the `posts` subdirectory, the value is `images:posts`.

```ts
{
  name: "image",
  type: "file",
  upload: "images:posts"
}
```

If it's not provided, the **first upload** value registered will be used. For
example:

```ts
// Register the "images" upload
cms.upload("images", "src:images");

cms.document("homepage", "src:index.yml", [
  "title: text",
  "content: markdown",
  "cover: file", // The `images` upload is used by default
]);
```

#### Save images in the same directory of the document

If you want to save the image in the same directory of the document, you may use
the `{document_dirname}` placeholder, that is replaced with the directory name
where the document is stored:

```ts
// Set a upload entity in `src:posts`
cms.upload("posts-images", "src:posts");

// Save the posts in the same directory (`src:posts`)
cms.collection({
  name: "posts",
  store: "src:posts/**/index.md",
  // Force a new folder per post
  documentName: "{title}/index.yml",
  fields: [
    "title: text",
    "content: markdown",
    {
      name: "cover",
      type: "file",
      // Store the image in the same folder of the post
      upload: "posts-images:{document_dirname}",
    },
  ],
});
```
