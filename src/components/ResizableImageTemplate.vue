<template>
	<node-view-wrapper as="span">
		<img v-bind="node.attrs" :draggable="isDraggable" :data-drag-handle="isDraggable" ref="resizableImg" />
	</node-view-wrapper>
</template>

<script>
import { NodeViewWrapper, nodeViewProps } from '@tiptap/vue-2';

export default {
	components: {
		NodeViewWrapper
	},

	props: nodeViewProps,

	data() {
		return {
			isResizing: false,
			lastMovement: 0,
			aspectRatio: 0
		};
	},

	computed: {
		isDraggable() {
			return this.node?.attrs?.isDraggable;
		}
	},

	mounted() {
		this.$refs.resizableImg.onload = () => {
			this.aspectRatio = this.$refs.resizableImg.naturalWidth / this.$refs.resizableImg.naturalHeight;
		};

		this.$refs.resizableImg.addEventListener('mousedown', (e) => {
			if (this.isDraggable) {
				return;
			}
			this.isResizing = true;
		});

		this.$refs.resizableImg.addEventListener('mousemove', (e) => {
			if (this.isResizing === false) {
				return;
			}

			let movement = Math.sqrt(Math.pow(e.offsetY, 2) + Math.pow(e.offsetX, 2));

			if (this.lastMovement > 0) {
				if (movement > this.lastMovement) {
					this.resizeAspectRatio(true);
				} else if (movement < this.lastMovement) {
					this.resizeAspectRatio(false);
				}
			}

			this.lastMovement = movement;
		});

		this.$refs.resizableImg.addEventListener('mouseup', (e) => {
			this.isResizing = false;
			this.lastMovement = 0;
		});
	},

	methods: {
		resizeAspectRatio(grow) {
			let calcW;
			let calcH;

			if (grow) {
				calcH = this.$refs.resizableImg.height + 1;
			} else {
				calcH = this.$refs.resizableImg.height - 1;
			}

			calcW = calcH * this.aspectRatio;

			this.updateAttributes({ width: calcW, height: calcH });
		}
	}
};
</script>
