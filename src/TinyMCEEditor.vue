<script setup lang="ts">
import Editor from '@tinymce/tinymce-vue'

const tinymceai_token_provider = async () => {
	await fetch(`https://demo.api.tiny.cloud/1/no-api-key/auth/random`, { method: "POST", credentials: "include" });
	return await fetch(`https://demo.api.tiny.cloud/1/no-api-key/jwt/tinymceai`, { credentials: "include" })
	  .then(resp => resp.text())
	  .then(token => ({ token }))
  }

</script>

<template>
  <Editor
    id="document-content"
    apiKey="no-api-key"
    :init="{
      plugins: 'tinymceai advlist anchor autolink charmap code fullscreen help image insertdatetime link lists media preview searchreplace table visualblocks wordcount',
      toolbar: 'tinymceai-chat tinymceai-quickactions tinymceai-review | undo redo | styles | bold italic underline strikethrough | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | link image',
      autoresize_overflow_padding: 50,
      content_style: 'body { line-height: 1.5; }',
      content_css: 'document',
      height: '100%',
      content_id: 'document-123',
      tinymceai_token_provider: tinymceai_token_provider,
    }"
  />
</template>

<style></style>