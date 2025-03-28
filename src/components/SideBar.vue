<template>
    <div class="sidebar">
        <h2>IMPEKABLE</h2>
        <ul class="sidebar-list">
            <li v-for="link in links" :key="link.text">
                <a :href="link.url" target="_blank" rel="noopener noreferrer">
                    {{ link.text }}
                </a>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'SideBar',
    data() {
        return {
            links: [
                { text: 'Home', url: 'https://example.com/home', icon: 'home' },
                { text: 'Dashboard', url: 'https://example.com/about', icon: 'info-circle' },
                { text: 'Dashboard', url: 'https://example.com/contact', icon: 'envelope' },
                { text: 'Products', url: 'https://example.com/home', icon: 'home' },
                { text: 'Invoices', url: 'https://example.com/about', icon: 'info-circle' },
                { text: 'Customers', url: 'https://example.com/contact', icon: 'envelope' },
                { text: 'Chat Room', url: 'https://example.com/home', icon: 'home' },
                { text: 'Calendar', url: 'https://example.com/about', icon: 'info-circle' },
            ]
        };
    }
};
</script>


<style scoped>
.sidebar {
    color: white;
    width: 260px;
    background: #43425D;

}

.sidebar h2 {
    font-family: serif;
    font-family: 'SourceSansPro-Bold', Arial, Helvetica Neue, Helvetica, sans-serif;
    font-size: 15px;
    padding: 25px 20px;
    background: #3c3c53;
}

.sidebar-list {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

.sidebar-list li {
    margin: 0;
    border-left: 5px solid transparent;
    transform: all .3s;
}

.sidebar-list li:hover {
    background: #3c3c53;
    border-color: #A3A0FB;
}

.sidebar-list a {
    display: block;
    padding: 20px 25px;
    text-decoration: none;
    color: white;
    font-size: 15px;
    width: 100%;
}

.sidebar-list .fa-icon {
    margin-right: 8px;
}
</style>