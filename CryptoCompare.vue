<template>
    <div :class="element_class" :id="element_id">

    </div>
</template>

<script>
export default {
    name: "Tabbed",
    props: {
        theme: {
            type: String,
            required: false,
            default: '{"General":{"headerText":"Crypto Converter"},"Form":{"labelFrom":"I Give","labelTo":"I Get"}}'
        },
        type: {
            required: false,
            default: 'tabbed'
        },
        element_class: {
            required: false,
            default: ''
        },
        tsyms: {
            required: false,
            default: 'NGN,USD,EUR,GBP'
        },
        fsyms: {
            required: false,
            default: 'BTC,ETH,BNB,LTC,XRP'
        },
        element_id: {
            required: false,
            default: 'live-price'
        },

    },
    mounted() {
        //crypto compare widget paths
        let paths = {
            'tabbed': `serve/v1/coin/multi?fsyms=${this.fsyms}&tsyms=${this.tsyms}`,
            'chart': `serve/v1/coin/chart?fsym=${this.fsyms}&tsym=${this.tsyms}`,
            'news': `serve/v1/coin/feed?fsym=${this.fsyms}&tsym=${this.tsyms}`,
            'list': `serve/v1/coin/list?fsym=${this.fsyms}&tsyms=${this.tsyms}`,
            'tiles': `serve/v1/coin/tiles?fsym=${this.fsyms}&tsyms=${this.tsyms}`,
            'horizontal': `serve/v1/coin/header?fsym=${this.fsyms}&tsyms=${this.tsyms}`,
            'summary': `serve/v1/coin/summary?fsym=${this.fsyms}&tsyms=${this.tsyms}`,
            'historical': `serve/v1/coin/histo_week?fsym=${this.fsyms}&tsym=${this.tsyms}`,
            'advanced': `serve/v3/coin/chart?fsym=${this.fsyms}&tsyms=${this.tsyms}`,
            'converter': `serve/v1/coin/converter?fsym=${this.fsyms}&tsyms=${this.tsyms}`,
            'headerv2': `serve/v2/coin/header?fsyms=${this.fsyms}&tsyms=${this.tsyms}`,
            'headerv3': `serve/v3/coin/header?fsyms=${this.fsyms}&tsyms=${this.tsyms}`,
            'chartscroller': `serve/v1/coin/chartscroller?fsyms=${this.fsyms}&tsyms=${this.tsyms}`,
        }
        let spt = document.createElement("script");
        spt.type = "text/javascript";
        spt.innerHTML = `
                        var baseUrl = "https://widgets.cryptocompare.com/";
                        var scripts = document.getElementsByTagName("script");
                        var embedder = scripts[scripts.length - 1];
                        var cccTheme = ${this.theme};
                        (function() {
                            var appName = encodeURIComponent(window.location.hostname);
                            if (appName == "") {
                                appName = "local";
                            }
                            var s = document.createElement("script");
                            s.type = "text/javascript";
                            s.async = true;
                            var theUrl = baseUrl + '${paths[this.type.toLowerCase()]}';
                            s.src = theUrl + (theUrl.indexOf("?") >= 0 ? "&" : "?") + "app=" + appName;
                            embedder.parentNode.appendChild(s);
                        })();
                    `;
        document.getElementById(this.element_id).appendChild(spt);
// var theUrl = baseUrl + 'serve/v1/coin/${this.widget[this.type]}?fsyms=${this.fsyms}&tsyms=${this.tsyms}';
    }
}
</script>

<style scoped>

</style>