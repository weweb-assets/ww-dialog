<template>
    <div
        :style="{
            '--transition-duration': animationDuration,
            '--transition-easing': content.animationEasing,
        }"
        role="dialog"
        class="ww-dialog"
    >
        <wwElement v-if="content.trigger" v-bind="content.triggerElement" role="dialog" @click="onTriggerClick()" />
        <Transition :name="transitionName">
            <div v-if="isOpen" class="ww-dialog-transition">
                <wwElement v-bind="content.contentElement" role="dialog" :style="contentStyle" />
            </div>
        </Transition>

        <template v-if="isOpen">
            <template v-if="!isEditing">
                <div v-if="content.clickOutsideCloses" class="pointer-capture" @click.stop="closeDialog()"></div>
                <div
                    v-else-if="content.preventInteractionsOutside && !content.overlay && !content.overlayClickCloses"
                    class="pointer-capture"
                    @click.stop="null"
                ></div>
            </template>

            <template v-if="content.overlay">
                <wwElement
                    ref="overlayElement"
                    v-bind="content.overlayElement"
                    role="dialog"
                    @click="handleOverlayClick()"
                />
            </template>
        </template>
    </div>
</template>

<script>
import { computed, ref, watch, onUnmounted } from 'vue';
import { useDialogStyle } from './composables/useDialogStyle';

export default {
    props: {
        content: { type: Object, required: true },
        uid: { type: String, required: true },
        /* wwEditor:start */
        wwEditorState: { type: Object, required: true },
        /* wwEditor:end */
    },
    emits: ['trigger-event'],
    setup(props, { emit }) {
        const { getModalStyle, getSheetStyle, getTransitionName } = useDialogStyle();

        const animationDuration = computed(() => {
            return props.content.animationDuration + 'ms';
        });

        const transitionName = computed(() => {
            return getTransitionName(props.content.animation, props.content.slideInDirection);
        });

        const { value: isOpen, setValue: setIsOpen } = wwLib.wwVariable.useComponentVariable({
            uid: props.uid,
            name: 'open',
            type: 'boolean',
            defaultValue: false,
            componentType: 'element',
        });

        const setDialogState = newValue => {
            setIsOpen(newValue);

            const eventName = newValue ? 'open' : 'close';
            emit('trigger-event', {
                name: eventName,
                event: {
                    open: newValue,
                },
            });

            emit('trigger-event', {
                name: 'change',
                event: {
                    open: newValue,
                },
            });
        };

        function toggleDialog() {
            setDialogState(!isOpen.value);
        }
        function openDialog() {
            setDialogState(true);
        }

        function closeDialog() {
            setDialogState(false);
        }

        function addEscapeListener() {
            wwLib.getFrontDocument().addEventListener('keydown', handleEscapeKey);
        }

        function removeEscapeListener() {
            wwLib.getFrontDocument().removeEventListener('keydown', handleEscapeKey);
        }

        function handleEscapeKey(event) {
            if (event.key === 'Escape') {
                onEscapeKeyDown();
            }
        }

        function onEscapeKeyDown() {
            if (isEditing.value || !props.content.escClose) {
                return;
            }
            closeDialog();
        }

        watch(
            () => isOpen.value,
            newValue => {
                if (props.content.preventScroll && !isEditing.value) {
                    const overflowValue = newValue ? 'hidden' : 'auto';
                    wwLib.getFrontDocument().body.style.overflow = overflowValue;
                    wwLib.getFrontDocument().documentElement.style.overflow = overflowValue;
                }
                if (newValue) {
                    addEscapeListener();
                } else {
                    removeEscapeListener();
                }
            }
        );

        const isEditing = computed(() => {
            /* wwEditor:start */
            return props.wwEditorState.editMode === wwLib.wwEditorHelper.EDIT_MODES.EDITION;
            /* wwEditor:end */
            // eslint-disable-next-line no-unreachable
            return false;
        });

        wwLib.wwElement.useRegisterElementLocalContext('dialog', ref({ isOpen }), {
            toggleDialog: {
                method: toggleDialog,
                editor: {
                    label: 'Toggle',
                    description: 'Toggle the dialog state.',
                },
            },
            openDialog: {
                method: openDialog,
                editor: {
                    label: 'Open',
                    description: 'Open the dialog.',
                },
            },
            closeDialog: {
                method: closeDialog,
                editor: {
                    label: 'Close',
                    description: 'Close the dialog.',
                },
            },
        });

        const contentStyle = computed(() => {
            let style = {};
            switch (props.content.type) {
                case 'modal':
                    style = getModalStyle(
                        props.content.sideModal,
                        props.content.align,
                        props.content.customPositionX,
                        props.content.customPositionY
                    );
                    break;
                case 'sheet':
                    style = getSheetStyle(props.content.sideSheet);
                    break;
            }
            return style;
        });

        function handleOverlayClick() {
            if (props.content.overlayClickCloses && !isEditing.value) {
                closeDialog();
            }
        }

        function handleOutsideClick() {
            if (props.content.clickOutsideCloses && !isEditing.value) {
                closeDialog();
            }
        }

        function onTriggerClick() {
            if (!props.content.triggerClickOpens || isEditing.value) {
                return;
            }

            toggleDialog();
        }

        onUnmounted(() => {
            removeEscapeListener();
            wwLib.getFrontDocument().body.style.overflow = 'auto';
            wwLib.getFrontDocument().documentElement.style.overflow = 'auto';
        });

        return {
            toggleDialog,
            openDialog,
            closeDialog,
            contentStyle,
            isOpen,
            transitionName,
            animationDuration,
            isEditing,
            onEscapeKeyDown,
            handleOverlayClick,
            onTriggerClick,
            handleOutsideClick,
        };
    },
};
</script>

<style lang="scss">
/* Fade */
.fade-transition-enter-active,
.fade-transition-leave-active {
    transition: opacity var(--transition-duration) var(--transition-easing) !important;
}

.fade-transition-enter-from,
.fade-transition-leave-to {
    opacity: 0;
}

/* Slide Left */
.slide-in-left-transition-enter-active,
.slide-in-left-transition-leave-active {
    transition: opacity var(--transition-duration) var(--transition-easing),
        transform var(--transition-duration) var(--transition-easing) !important;
}

.slide-in-left-transition-enter-from,
.slide-in-left-transition-leave-to {
    opacity: 0;
    transform: translateX(-20px);
}

/* Slide Right */
.slide-in-right-transition-enter-active,
.slide-in-right-transition-leave-active {
    transition: opacity var(--transition-duration) var(--transition-easing),
        transform var(--transition-duration) var(--transition-easing) !important;
}

.slide-in-right-transition-enter-from,
.slide-in-right-transition-leave-to {
    opacity: 0;
    transform: translateX(20px);
}

/* Slide Top */
.slide-in-top-transition-enter-active,
.slide-in-top-transition-leave-active {
    transition: opacity var(--transition-duration) var(--transition-easing),
        transform var(--transition-duration) var(--transition-easing) !important;
}

.slide-in-top-transition-enter-from,
.slide-in-top-transition-leave-to {
    opacity: 0;
    transform: translateY(-20px);
}

/* Slide Bottom */
.slide-in-bottom-transition-enter-active,
.slide-in-bottom-transition-leave-active {
    transition: opacity var(--transition-duration) var(--transition-easing),
        transform var(--transition-duration) var(--transition-easing) !important;
}

.slide-in-bottom-transition-enter-from,
.slide-in-bottom-transition-leave-to {
    opacity: 0;
    transform: translateY(20px);
}

/* Zoom */
.zoom-transition-enter-active,
.zoom-transition-leave-active {
    transition: transform var(--transition-duration) var(--transition-easing) !important;
}

.zoom-transition-enter-from,
.zoom-transition-leave-to {
    transform: scale(0);
}

.pointer-capture {
    position: fixed;
    top: 0;
    left: 0;
    width: 100dvw;
    height: 100dvh;
    pointer-events: auto;
}
</style>
