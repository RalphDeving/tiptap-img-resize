<template>
	<div>
		<editor-content :editor="editor" />
		<button v-show="showButton" @click="toggleResize">{{ isDraggable ? 'Dragging' : 'Resizing' }}</button>
	</div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-2';
import StarterKit from '@tiptap/starter-kit';
import ResizableImage from './resizable-image.js';

export default {
	components: {
		EditorContent
	},

	props: {
		value: {
			type: String,
			default: ''
		}
	},

	data() {
		return {
			editor: null
		};
	},

	computed: {
		showButton() {
			return this.editor?.state?.selection?.node?.type?.name === 'ResizableImage';
		},
		isDraggable() {
			return this.editor?.state?.selection?.node?.attrs?.isDraggable;
		}
	},

	watch: {
		value(value) {
			const isSame = this.editor.getHTML() === value;

			if (isSame) {
				return;
			}

			this.editor.commands.setContent(value, false);
		}
	},

	mounted() {
		this.editor = new Editor({
			content: this.value,
			extensions: [
				StarterKit,
				ResizableImage.configure({
					inline: true
				})
			],
			onUpdate: () => {
				this.$emit('input', this.editor.getHTML());
			}
		});
	},

	methods: {
		toggleResize() {
			this.editor.chain().focus().toggleResizable().run();
		}
	},

	beforeDestroy() {
		this.editor.destroy();
	}
};
</script>

<style>
.ProseMirror {
	outline: 1px solid;
}
</style>
