<script>
  // @ts-nocheck

  import { onMount } from "svelte";

  /**
   * @type {string | Blob | null}
   */
  let selectedFile = null;

  /**
   * @param {{ target: { files: (string | Blob | null)[]; }; }} event
   */
  function handleFileSelect(event) {
    selectedFile = event.target.files[0];
    console.log(selectedFile);
  }

  function uploadFile() {
    if (selectedFile) {
      console.log(selectedFile);
      const formData = new FormData();
      formData.append("file", selectedFile);
      fetch("/upload", {
        method: "POST",
        body: formData,
      })
        .then((response) => response.json())
        .then((data) => {
          console.log("File uploaded successfully:", data);
        })
        .catch((error) => {
          console.error("File upload failed:", error);
        });
    }
  }

  onMount(() => {
    console.log("FileUploader component mounted");
  });
</script>

<h1>File Uploader</h1>

<input type="file" on:change={handleFileSelect} />

<button on:click={uploadFile}>Upload</button>
