<template>
    <div class="form-group">
        <div class='input-group date'>
            <input type='text' class="form-control" />
            <span class="input-group-addon">
                <span class="glyphicon glyphicon-calendar"/>
            </span>
        </div>
    </div>
</template>

<script>
import $ from 'jquery';
import 'eonasdan-bootstrap-datetimepicker';
export default {
    name : 'date-time',
    props : { options : {}, value : { default : '' } },
    mounted() {
        /**
            The jquery selector gets the root element the vue component is attached to,
            then filters down to the first text input, which in this template, will always
            be the datetime input.

            It then initialises it with any options passed through the options prop,
            emits an input event when the date input changes, allowing the component to
            be bound to vue reactivity system via v-model. Lastly, any initial value passed
            through the props will initialise the start value for the input.
        */
        let el = $(this.$el).find('input').first();
	    let vm = this;
        el.datetimepicker(vm.options);
	    el.on('dp.change', (val) => {
	        vm.$emit('input', val);
        });
	    el.data("DateTimePicker").date(vm.value);
    }
}
</script>
<style scope>
@import '../node_modules/eonasdan-bootstrap-datetimepicker/build/css/bootstrap-datetimepicker.css';
</style>
