<template>
    <div :class='styleClass' style="width: 100%;">
        <span v-if="label" class="input-group-addon">
            {{label}}
        </span>
        <input type='text' class="form-control"  title=""/>
        <span v-if="icon" class="input-group-addon">
            <span class="glyphicon glyphicon-calendar"/>
        </span>
    </div>
</template>

<script>
import $ from 'jquery';
import 'eonasdan-bootstrap-datetimepicker';
import moment from 'moment';
export default {
    name : 'date-time',
    props : {
        options : {},
        value : { default : '' },
        class : {},
        icon : { default : false },
        label : {}
    },
    data() {
        return {
            el : $(this.$el).find('input').first()
        }
    },
    computed : {
        styleClass() {
            let classes = {
                'input-group' : true,
                'date' : true
            };
            for (let style in this.class) {
                classes[style] = true;
            }
            return classes;
        }
    },
    mounted() {
        /**
            The jquery selector gets the root element that the vue component is attached to,
            then filters down to the first text input, which in this template, will always
            be the datetime input.

            It then initialises it with any options passed through the options prop,
            emits an input event when the date input changes, allowing the component to
            be bound to vue reactivity system via v-model. Lastly, any initial value passed
            through the props will initialise the start value for the input.
        */
        this.el.datetimepicker(this.options);
	    this.el.on('dp.change', (val) => {
	        /**
	         * Datetime picker plugin returns false if no date is set, this is not
	         * really an accurate representation of the inputs value, so we report
	         * this back to v-model as null.
	         */
	        if (! val) {
	            val = null;
	        }
	        this.$emit('input', val.date);
        });
        this.updateInput(this.value);
    },
    methods : {
        formatDate(val) {
            let date;
            if (val === "") {
                date = null;
            } else {
                date = moment(val);
                if (! date.isValid()) {
                    date = null;
                }
            }
            return date;
        },
        updateInput(val) {
            let date = this.formatDate(val);
            try {
                this.el.data("DateTimePicker").date(date);
            } catch (e) {
                console.error(e);
                this.el.data("DateTimePicker").date(null);
            }
        }
    },
    watch : {
        value : function(val) {
            this.updateInput(val);
        }
    }
}
</script>
<style scoped>
@import '../node_modules/eonasdan-bootstrap-datetimepicker/build/css/bootstrap-datetimepicker.css';
</style>
