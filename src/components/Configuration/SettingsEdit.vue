<template>
    <wwEditorFormRow required label="Application ID">
        <template #append-label>
            <a class="ww-editor-link ml-2" href="https://www.algolia.com/account/api-keys/all" target="_blank">
                Find it here
            </a>
        </template>
        <wwEditorInputRow
            type="query"
            placeholder="Enter a value"
            :model-value="settings.publicData.applicationId"
            @update:modelValue="changeApplicationId"
        />
    </wwEditorFormRow>
    <wwEditorInputRow
        label="Search-Only API Key"
        required
        type="query"
        placeholder="Enter a value"
        :model-value="settings.publicData.apiKey"
        @update:modelValue="changeApiKey"
    />
    <wwEditorFormRow required label="Admin API Key">
        <wwEditorInputText
            type="text"
            placeholder="********"
            :model-value="settings.privateData.apiKey"
            :style="{ '-webkit-text-security': isKeyVisible ? 'none' : 'disc' }"
            large
            @update:modelValue="changePrivateApiKey"
        />
    </wwEditorFormRow>
    <div class="flex items-center">
        <wwEditorInputSwitch v-model="isKeyVisible" />
        <span class="ml-2 body-2">Show private api key</span>
    </div>
</template>

<script>
export default {
    props: {
        plugin: { type: Object, required: true },
        settings: { type: Object, required: true },
    },
    emits: ['update:settings'],
    data() {
        return {
            isKeyVisible: false,
        };
    },
    methods: {
        changeApplicationId(applicationId) {
            this.$emit('update:settings', {
                ...this.settings,
                publicData: { ...this.settings.publicData, applicationId },
            });
            this.$nextTick(this.loadInstance);
        },
        changeApiKey(apiKey) {
            this.$emit('update:settings', {
                ...this.settings,
                publicData: { ...this.settings.publicData, apiKey },
            });
            this.$nextTick(this.loadInstance);
        },
        changePrivateApiKey(apiKey) {
            this.$emit('update:settings', {
                ...this.settings,
                privateData: { ...this.settings.privateData, apiKey },
            });
            this.$nextTick(this.loadInstance);
        },
        loadInstance() {
            this.plugin.load(this.settings.publicData.applicationId, this.settings.privateData.apiKey);
        },
    },
};
</script>
