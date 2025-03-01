<template>
    <div
        :class="$slider.thumbWrapperClasses"
        :style="wrapperStyle">
        <o-tooltip
            :label="formattedValue"
            :variant="variant"
            :always="dragging || isFocused || tooltipAlways"
            :active="!disabled && tooltip">
            <div
                v-bind="$attrs"
                :class="$slider.thumbClasses"
                :tabindex="disabled ? false : 0"
                @mousedown="onButtonDown"
                @touchstart="onButtonDown"
                @focus="onFocus"
                @blur="onBlur"
                @keydown.left.prevent="onLeftKeyDown"
                @keydown.right.prevent="onRightKeyDown"
                @keydown.down.prevent="onLeftKeyDown"
                @keydown.up.prevent="onRightKeyDown"
                @keydown.home.prevent="onHomeKeyDown"
                @keydown.end.prevent="onEndKeyDown">
                <span v-if="indicator">{{ formattedValue }}</span>
            </div>
        </o-tooltip>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

import Tooltip from '../tooltip/Tooltip.vue'

import { getOptions } from '../../utils/config'
import { getValueByPath } from '../../utils/helpers'

export default defineComponent({
    name: 'OSliderThumb',
    components: {
        [Tooltip.name]: Tooltip
    },
    configField: 'slider',
    inheritAttrs: false,
    inject: ['$slider'],
    emits: ['update:modelValue', 'dragstart', 'dragend'],
    props: {
        modelValue: {
            type: Number,
            default: 0
        },
        variant: {
            type: String,
            default: ''
        },
        tooltip: {
            type: Boolean,
            default: true
        },
        indicator: {
            type: Boolean,
            default: false
        },
        customFormatter: Function,
        format: {
            type: String,
            default: 'raw',
            validator: (value: string) => {
                return [
                    'raw',
                    'percent'
                ].indexOf(value) >= 0
            }
        },
        locale: {
            type: [String, Array],
            default: () => {
                return getValueByPath(getOptions(), 'locale')
            }
        },
        tooltipAlways: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            isFocused: false,
            dragging: false,
            startX: 0,
            startPosition: 0,
            newPosition: null,
            oldValue: this.modelValue
        }
    },
    computed: {
        disabled() {
            return this.$parent.disabled
        },
        max() {
            return this.$parent.max
        },
        min() {
            return this.$parent.min
        },
        step() {
            return this.$parent.step
        },
        precision() {
            return this.$parent.precision
        },
        currentPosition() {
            return `${(this.value - this.min) / (this.max - this.min) * 100}%`
        },
        wrapperStyle() {
            return { left: this.currentPosition }
        },
        formattedValue() {
            if (typeof this.customFormatter !== 'undefined') {
                return this.customFormatter(this.value)
            }
            if (this.format === 'percent') {
                return new Intl.NumberFormat(
                    this.locale,
                    {
                        style: 'percent'
                    }
                ).format(((this.value - this.min)) / (this.max - this.min))
            }
            return new Intl.NumberFormat(this.locale).format(this.value)
        }
    },
    methods: {
        onFocus() {
            this.isFocused = true
        },
        onBlur() {
            this.isFocused = false
        },
        onButtonDown(event) {
            if (this.disabled) return
            event.preventDefault()
            this.onDragStart(event)
            if (typeof window !== 'undefined') {
                document.addEventListener('mousemove', this.onDragging)
                document.addEventListener('touchmove', this.onDragging)
                document.addEventListener('mouseup', this.onDragEnd)
                document.addEventListener('touchend', this.onDragEnd)
                document.addEventListener('contextmenu', this.onDragEnd)
            }
        },
        onLeftKeyDown() {
            if (this.disabled || this.value === this.min) return
            this.newPosition = parseFloat(this.currentPosition) -
                this.step / (this.max - this.min) * 100
            this.setPosition(this.newPosition)
            this.$parent.emitValue('change')
        },
        onRightKeyDown() {
            if (this.disabled || this.value === this.max) return
            this.newPosition = parseFloat(this.currentPosition) +
                this.step / (this.max - this.min) * 100
            this.setPosition(this.newPosition)
            this.$parent.emitValue('change')
        },
        onHomeKeyDown() {
            if (this.disabled || this.value === this.min) return
            this.newPosition = 0
            this.setPosition(this.newPosition)
            this.$parent.emitValue('change')
        },
        onEndKeyDown() {
            if (this.disabled || this.value === this.max) return
            this.newPosition = 100
            this.setPosition(this.newPosition)
            this.$parent.emitValue('change')
        },
        onDragStart(event) {
            this.dragging = true
            this.$emit('dragstart')
            if (event.type === 'touchstart') {
                event.clientX = event.touches[0].clientX
            }
            this.startX = event.clientX
            this.startPosition = parseFloat(this.currentPosition)
            this.newPosition = this.startPosition
        },
        onDragging(event) {
            if (this.dragging) {
                if (event.type === 'touchmove') {
                    event.clientX = event.touches[0].clientX
                }
                const diff = (event.clientX - this.startX) / this.$parent.sliderSize() * 100
                this.newPosition = this.startPosition + diff
                this.setPosition(this.newPosition)
            }
        },
        onDragEnd() {
            this.dragging = false
            this.$emit('dragend')
            if (this.value !== this.oldValue) {
                this.$parent.emitValue('change')
            }
            this.setPosition(this.newPosition)
            if (typeof window !== 'undefined') {
                document.removeEventListener('mousemove', this.onDragging)
                document.removeEventListener('touchmove', this.onDragging)
                document.removeEventListener('mouseup', this.onDragEnd)
                document.removeEventListener('touchend', this.onDragEnd)
                document.removeEventListener('contextmenu', this.onDragEnd)
            }
        },
        setPosition(percent) {
            if (percent === null || isNaN(percent)) return
            if (percent < 0) {
                percent = 0
            } else if (percent > 100) {
                percent = 100
            }
            const stepLength = 100 / ((this.max - this.min) / this.step)
            const steps = Math.round(percent / stepLength)
            let value = steps * stepLength / 100 * (this.max - this.min) + this.min
            value = parseFloat(value.toFixed(this.precision))
            this.$emit('update:modelValue', value)
            if (!this.dragging && value !== this.oldValue) {
                this.oldValue = value
            }
        }
    }
})
</script>
