<script>
import BaseComponentMixin from '../../utils/BaseComponentMixin.js'
import TabbedChildMixin from '../../utils/TabbedChildMixin.js'

/**
 * @displayName Step Item
 */
export default {
    name: 'OStepItem',
    mixins: [BaseComponentMixin, TabbedChildMixin('step')],
    configField: 'steps',
    props: {
        /** Step marker content (when there is no icon) */
        step: [String, Number],
        /** Default style for the step, optional This will override parent type. Could be used to set a completed step to "success" for example */
        variant: [String, Object],
        /** Item can be used directly to navigate. If undefined, previous steps are clickable while the others are not */
        clickable: {
            type: Boolean,
            default: undefined
        },
        itemClass: [String, Function, Array],
        itemHeaderClass: [String, Function, Array],
        itemHeaderActiveClass: [String, Function, Array],
        itemHeaderPreviousClass: [String, Function, Array]
    },
    computed: {
        elementClasses() {
            return [
                this.computedClass('itemClass', 'o-steps__item')
            ]
        },
        itemClasses() {
            return [
                this.headerClass,
                this.computedClass('itemHeaderClass', 'o-steps__nav-item'),
                { [this.computedClass('itemHeaderActiveClass', 'o-steps__nav-item-active')]: this.isActive },
                { [this.computedClass('itemHeaderPreviousClass', 'o-steps__nav-item-previous')]: this.parent.activeItem.index > this.index }
            ]
        }
    }
}
</script>
