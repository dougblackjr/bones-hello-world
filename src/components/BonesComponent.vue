<template>
    <div class="border border-indigo-800 border-2 mt-8 p-4">
        <div class="flex flex-wrap -mx-4 -mb-4 md:mb-0 items-center justify-center">
            <div class="w-full md:w-5/6 px-4 mb-4 md:mb-0">
                <div class="mb-6">
                    <label for="queryString" class="block text-sm font-medium mb-2">Route</label>
                    <input v-model="route" class="block w-full px-4 py-3 mb-2 text-sm placeholder-gray-500 bg-white border rounded" type="text" name="" placeholder='/api' />
                    <label for="queryString" class="block text-sm font-medium mb-2">API Key</label>
                    <input v-model="apiKey" class="block w-full px-4 py-3 mb-2 text-sm placeholder-gray-500 bg-white border rounded" type="text" name="" placeholder='12345' />
                    <label for="queryString" class="block text-sm font-medium mb-2">Query String without Query String</label>
                    <input v-model="queryString" class="block w-full px-4 py-3 mb-2 text-sm placeholder-gray-500 bg-white border rounded" type="text" name="" placeholder='channel="blog"&status="draft"' />
                </div>
            </div>
            <div class="w-full md:w-1/6 px-4 mb-4 md:mb-0">
                <button
                    v-on.prevent="getPosts"
                    class="inline-block w-full md:w-auto px-6 py-3 font-medium text-white bg-indigo-500 hover:bg-indigo-600 rounded transition duration-200"
                >Submit</button>
            </div>
        </div>
        <section class="py-8">
            <div class="container px-4 mx-auto">
                <div class="flex flex-wrap -m-4">
                    <div class="w-full grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 p-4">
                        <!-- BEGIN CARDS -->
                        <div
                            v-for="(entry, idx) in entries"
                            :key="idx"
                            class="p-6 mb-4 bg-white rounded shadow w-full">
                            <div class="flex justify-between items-center mb-6">
                                <span class="inline-block py-1 px-2 bg-blue-50 text-xs text-blue-500 rounded-full">{{ entry.status }}</span>
                            </div>
                            <div class="mb-4">
                                <h3 class="mb-2 font-medium">{{ entry.title }}</h3>
                                <p class="text-sm text-gray-500"></p>
                            </div>
                            <div class="flex justify-between overflow-auto" style="max-height: 200px;">
                                <p class="text-gray-700">{{ computedFields(entry) }}</p>
                            </div>
                        </div>
                        <!-- END CARDS -->
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
    export default {
        components: {},
        props: {},
        data() {
            return {
                success: false,
                route: '/api',
                apiKey: '',
                queryString: '',
                entries: []
            }
        },
        methods: {
            computedFields(post) {
                return JSON.stringify(post, undefined, 2)
            },
            getPosts() {
                let self = this
                const urlString = this.getUrl(),
                    headers = {
                        'Content-Type': 'application/json',
                        'Access-Control-Allow-Origin': '*'
                    };

                const settings = {
                    method: 'GET',
                    mode: 'cors',
                    headers: headers
                }
                fetch(urlString, settings)
                    .then(response => response.json())
                    .then(data => {
                        self.success = data.success
                        self.entries = data.entries
                    })
            },
            getUrl() {
                const currentUrl = `${window.location.protocol}//${window.location.hostname}${this.route}`
                let url = new URL(currentUrl);
                let params = url.searchParams;

                if(this.apiKey) {
                    params.append('api_key', this.apiKey)
                }

                let urlString = url.toString()

                if(this.queryString) {
                    if(urlString.includes('?')) {
                        urlString = `${urlString}&${this.queryString}`
                    } else {
                        urlString = `${urlString}?${this.queryString}`
                    }
                }

                return urlString
            }
        },
        mounted() {
            this.getPosts()
        }
    };
</script>