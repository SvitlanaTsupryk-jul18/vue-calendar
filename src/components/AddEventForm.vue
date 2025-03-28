<template>
    <div v-if="isForm" class="app-form">
        <p class="form-close" @click="cancelForm"> <img alt="alt message" class="your-logo-css-class"
                src="@/assets/close.svg"></p>
        <form @submit.prevent="onSubmit" novalidate>
            <div class="form-group">
                <input v-model="form.title" placeholder="event name" required maxlength="30" @blur="validateText" />
                <span v-if="errors.title" class="form-error">{{ errors.title }}</span>
            </div>
            <div class="form-group">
                <input v-model="form.date" placeholder="event date" type="date" required :min="form.minDate"
                    @blur="validateDate" />
                <span v-if="errors.date" class="form-error">{{ errors.date }}</span>
            </div>
            <div class="form-group">
                <input v-model="form.time" placeholder="event time" type="time" required @blur="validateTime"
                    step="1800" />
                <span v-if="errors.time" class="form-error">{{ errors.time }}</span>
            </div>
            <div class="form-group">
                <label for="color">Color:</label>
                <input v-model="form.color" type="color" />
            </div>
            <div class="form-buttons">
                <button type="button" @click="cancelForm">Cancel</button>
                <button type="submit">Save</button>
            </div>
        </form>
    </div>
</template>

<script>

export default {
    name: 'AddEventForm',
    props: {
        isForm: {
            type: Boolean,
            required: true
        }
    },
    data() {
        return {
            form: {
                title: '',
                date: '',
                time: '',
                color: '#3b86ff',
                minDate: new Date().toISOString().replace(/T.*$/, ''),
            },
            errors: {
                text: null,
                date: null,
                time: null
            }
        };
    },
    methods: {
        validateText() {
            if (!this.form.title) {
                this.errors.title = 'Title is required.';
            } else {
                this.errors.title = null;
            }
        },
        validateDate() {
            if (!this.form.date) {
                this.errors.date = 'Date is required.';
            } else {
                this.errors.date = null;
            }
        },
        validateTime() {
            if (!this.form.date) {
                this.errors.time = 'Time is required.';
            } else {
                this.errors.time = null;
            }
        },
        onSubmit() {
            this.validateText();
            this.validateDate();
            this.validateTime();
            if (!this.errors.text && !this.errors.date && !this.errors.time) {
                let newEvent = {
                    title: this.form.title,
                    date: this.form.date,
                    time: this.form.time,
                    color: this.form.color,
                }
                this.$emit('submitted', newEvent);
                this.resetForm();
            }
        },
        resetForm() {
            this.form.text = '';
            this.form.date = '';
            this.form.time = '';
            this.errors = {
                text: null,
                date: null,
                time: null
            };
        },
        cancelForm() {
            this.$emit('formCancelled');
        },
    },
};
</script>