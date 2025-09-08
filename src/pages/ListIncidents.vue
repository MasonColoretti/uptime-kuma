<template :class="{'dark': this.isDarkMode()}">
    <div class="incident-report-container">
        <h1>{{ $t("Incident Reports") }}</h1>
        <incident-list hideViewHistoryPage :slug="$route.params.slug" />
    </div>
</template>

<script>
import IncidentList from "../components/IncidentList.vue";
import axios from "axios";

export default {
    components: { IncidentList },
    data() {
        return {
            slug: null,
            theme: "",
        };
    },

    async mounted() {
        this.slug = this.$route.params.slug;
        
        let statusPageConfig = await axios.get("/api/status-page/" + this.slug)
        this.theme = statusPageConfig.data.config.theme;
        
        if (this.theme === 'dark') {
            document.body.classList.remove('light');
            document.body.classList.add('dark');
        } else if (this.theme === 'auto') {
            if (window.matchMedia("(prefers-color-scheme: dark)").matches) {
                document.body.classList.remove('light');
                document.body.classList.add("dark");
            }
        }
    },
    
    methods: {
        isDarkMode () {
            return this.theme === "dark";
        }
    }
};
</script>
<style>
.incident-report-container {
    padding: 32px;
}
</style>

