# multipart-blob-upload-example
A Proof-of-Concept example of how to store large files in TrueVault using BLOBs &amp; Documents to manage multi-part upload and downloads.

## Example Only 
This is Proof Of Concept code intended to demonstrate how one could upload large files to TrueVault by splitting them into several small files. This is not intended as a reference for production code. We recommend thorough error handling and retry logic for production code.

# Overview
  
 This demo shows you how you can take a file from a form, split it into many smaller chunks, and upload those chunks to TrueVault in parallel. It then shows the reverse process: downloading the chunks and stitching them back together in order.
  
The uploading code follows these steps:
1) Select a file to upload
1) Split that file into moderately sized chunks (configurable below)
1) Upload each chunk in parallel
1) Collect BLOB ids for each chunk, and save them in a metadata Document (order matters!)
1) Store the metadata Document
 
To retrieve the saved file, we follow these steps:
1) Load the metadata Document
2) Download each BLOB based on the ids in the metadata object (order matters!)
3) Combine the resulting data into a JavaScript `Blob`
4) Save that JS Blob to disk with the original file name (not necessary if you show file inline).


# Usage
To use this sample code, you'll need a TrueVault account where you can create a Vault, then create a user who can Create and Read all Documents/BLOBs in that Vault. Here's an example minimal policy that should work (put your vault id in place of the 000s):

```
[{
   "Resources": [
       "Vault::00000000-0000-0000-0000-00000000000::Document::,
       "Vault::00000000-0000-0000-0000-00000000000::Blob::,
   ],
   "Activities": "CR"
},{
    "Resources": [
       "Vault::00000000-0000-0000-0000-00000000000::Document::.*,
       "Vault::00000000-0000-0000-0000-00000000000::Blob::.*,
   ],
   "Activities": "R"
}]
```

You need to generate an API Key for that user in the console or through the API, then enter the API Key and Vault Id into the form to test.


