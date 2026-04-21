<script setup lang="ts">
import Editor from '@tinymce/tinymce-vue'

const tokenProvider = async () => {
  await fetch('https://demo.api.tiny.cloud/1/unique-tinymce-key/auth/random', { method: 'POST', credentials: 'include' });
  return { token: await fetch('https://demo.api.tiny.cloud/1/unique-tinymce-key/jwt/tinymceai', { credentials: 'include' }).then(r => r.text()) };
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
      tinymceai_token_provider: tokenProvider,
    }"
  />
</template>

<style></style>
