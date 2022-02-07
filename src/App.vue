<template>
	<div id="app">
		<TiptapEditor v-model="html"></TiptapEditor>

		<textarea v-model="html" cols="60" rows="3"></textarea>
	</div>
</template>

<script>
import TiptapEditor from './components/TiptapEditor.vue';

export default {
	name: 'App',
	components: {
		TiptapEditor
	},

	data() {
		return {
			html: '<h1>Hello There</h1>\n<p>Resizable image:</p>\n<img src="https://source.unsplash.com/8xznAGy4HcY/800x400" width="240" height="120" />'
		};
	}
};
</script>
