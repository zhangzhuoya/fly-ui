<template>
    <template v-if="visiable">
      <Teleport to="body">
        <div class="fly-dialog-overlay" @click="clickOverlay"></div>
        <div class="fly-dialog-wrapper">
            <div class="fly-dialog">
            <header><slot name="title"></slot> <span class="fly-dialog-close" @click="closeDialog"></span></header>
            <main>
                <slot name="content"></slot>
            </main>
            <footer>
                <Button level="main" @click="okCloseDialog">OK</Button>
                <Button @click="cancelCloseDialog">Cancel</Button>
            </footer>
        </div>
        </div>
      </Teleport>
    </template>
</template>
<script lang="ts">
import Button from "../button/FlyButton.vue";
export default {
    components: {
        Button,
    },
    props: {
        visiable: {
            type: Boolean
        },
        ok: {
            type: Function
        },
        cancel: {
            type: Function
        },
        closeOverlay: {
            type: Boolean,
            default: true,
        }
    },
    setup(props,context) {
        const okCloseDialog = ()=>{
            context.emit('update:visiable', false);
        }
        const cancelCloseDialog = ()=>{
            if(props?.cancel()) {
                context.emit('update:visiable', !props.visiable);
            }
        }
        const closeDialog = ()=>{
            if(props?.ok()) {
                context.emit('update:visiable', !props.visiable);
            }
        }
        const clickOverlay = ()=>{
            if(props.closeOverlay) {
                context.emit('update:visiable', false);
            }
        }
        return {
            okCloseDialog,cancelCloseDialog,closeDialog,clickOverlay
        }

    }
};
</script>

<style lang="scss">
$radius: 4px;
$border-color: #d9d9d9;
.fly-dialog {
    background: white;
    border-radius: $radius;
    box-shadow: 0 0 3px fade_out(black, 0.5);
    min-width: 15em;
    max-width: 90%;
    &-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: fade_out(black, 0.5);
    z-index: 10;
    }
    &-wrapper {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 11;
    }
    >header {
    padding: 12px 16px;
    border-bottom: 1px solid $border-color;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 20px;
    }
    >main {
    padding: 12px 16px;
    }
    >footer {
    border-top: 1px solid $border-color;
    padding: 12px 16px;
    text-align: right;
    }
    &-close {
    position: relative;
    display: inline-block;
    width: 16px;
    height: 16px;
    cursor: pointer;
    &::before,
    &::after {
        content: '';
        position: absolute;
        height: 1px;
        background: black;
        width: 100%;
        top: 50%;
        left: 50%;
    }
    &::before {
        transform: translate(-50%, -50%) rotate(-45deg);
    }
    &::after {
        transform: translate(-50%, -50%) rotate(45deg);
    }
    }
}
</style>