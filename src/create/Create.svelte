<script>
  import { navigate } from 'svelte-routing';
  import BackButtonRow from '../common/BackButtonRow.svelte';
  import BookCover from '../common/BookCover.svelte';
  import Button from '../common/Button.svelte';
  import Header from '../common/Header.svelte';
  import { httpPost } from '../common/api.js';
  import TextInput from './TextInput.svelte';
  let title = '',
    author = '',
    cover = '',
    about = '';

  $: book = { title, author, cover, about };

  async function handleSubmit(event) {
    function getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }
    const newBook = {
      ...book,
      cover: cover || 'https://via.placeholder.com/150',
      variation: getRandomInt(0, 2),
      favorite: false,
    };

    const { ok } = await httpPost('/', newBook);
    if (ok) {
      navigate('/');
    }
  }
</script>

<BackButtonRow />

<Header element="h1" size="large">Create</Header>

<form on:submit|preventDefault={handleSubmit}>
  <div class="fields">
    <TextInput label="Title" bind:value={title} />
    <TextInput label="Author" bind:value={author} />
    <TextInput label="Cover URL" bind:value={cover} />
    <TextInput label="About" bind:value={about} multiline />
    <div>
      <Button>Save</Button>
    </div>
  </div>

  <div>
    <Header>Preview</Header>
    <div class="preview">
      <BookCover {book} />
    </div>
  </div>
</form>

<style>
  label {
    font-weight: var(--typeWeightBold);
    text-transform: uppercase;
  }
  span {
    padding: 0 0 var(--spacingSmall) var(--spacingLarge);
  }
  input,
  textarea {
    display: block;
    background: #f8ecde;
    border-radius: 25px;
    border: 2px solid var(--colorFg);
    padding: 0.75rem var(--spacingSmall) 0.75rem var(--spacingLarge);
    width: 100%;
  }
  textarea {
    height: 8rem;
  }

  form {
    display: grid;
    grid-auto-rows: auto;
    grid-template-columns: 1fr;
    gap: var(--spacingXLarge);
  }
  .fields {
    display: grid;
    grid-auto-rows: auto;
    gap: var(--spacingMedium);
  }
  .preview {
    display: grid;
    grid-template-columns: minmax(20vw, 10rem);
    grid-template-rows: minmax(32vw, 16rem);
  }
  @media (min-width: 48rem) {
    form {
      grid-template-columns: 60vw 20vw;
    }
  }
</style>
