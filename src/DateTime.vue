<template>
    <div class='col-sm-6'>
        <div class="form-group">
            <div class='input-group date'>
                <input type='text' class="form-control" />
                <span class="input-group-addon">
                    <span class="glyphicon glyphicon-calendar"/>
                </span>
            </div>
        </div>
    </div>
</template>

<script>
import $ from 'jquery';
import 'eonasdan-bootstrap-datetimepicker';
export default {
    name : 'date-time',
    props : { title : {}, options : {}, value : { default : '' } },
    mounted() {
        $('.date').datetimepicker(this.options)
             .on('dp-change', (val) => {
                this.$emit('input', val);
             });
        $('.date').value(this.value);
    }
}
</script>
<style scope>
@import '../node_modules/eonasdan-bootstrap-datetimepicker/build/css/bootstrap-datetimepicker.css';
</style>
