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
        let el = $(this.$el).find('input').first();
        el.datetimepicker(this.options);
	    el.on('dp.change', (val) => {
	        this.$emit('input', val.date);
        });
    },
    watch : {
        value : function(val) {
            let el = $(this.$el).find('input').first();
            let date;
            if (val === "") {
                date = null;
            } else {
                date = moment(val);
            }
            if (! date.isValid()) {
                date = null;
            }
            try {
                el.data("DateTimePicker").date(date);
            } catch (e) {
                console.error(e);
                el.data("DateTimePicker").date(null);
            }
        }
    }
}
</script>
<style scoped>
@import '../node_modules/eonasdan-bootstrap-datetimepicker/build/css/bootstrap-datetimepicker.css';
</style>
