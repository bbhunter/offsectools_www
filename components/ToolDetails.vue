<template>
    <div id="tool-details">
        <div style="left:-35px;position:relative;">
            <Sharer :tool=tool></Sharer>
        </div>
        <div class="row">
            <div class="col-xl-8 col-lg-10 col-sm-12">
                <div class="row">
                    <div class="col-xl-9 col-lg-10 col-sm-11 col-11 position-relative overflow-hidden pe-0">
                        <template v-if="tool.featured">
                            <Ribbon rstyle="warning" rtext="featured"></Ribbon>
                        </template>
                        <template v-if="tool.sponsored">
                            <Ribbon rstyle="danger" rtext="sponsor"></Ribbon>
                        </template>
                        <img :alt="tool.nicename" :title="tool.nicename" :src="$config.ASSETS_URL+'/tools/'+tool.images[0]" id="tool-picture" />
                        <!-- <nuxt-img :alt="tool.nicename+' on '+$config.APP_NAME" :src="$config.ASSETS_URL+'/tools/'+tool.images[0]" placeholder="/img/default-tool.png" class="rounded tool-picture img-fluid" /> -->
                    </div>
                </div>
                <template v-if="tool.images.length > 1">
                    <div class="row mt-3">
                        <div class="col">
                            <img @mouseover="setToolImage(img)" @click="setToolImage(img)" :alt="tool.nicename" :title="tool.nicename" :src="$config.ASSETS_URL+'/tools/'+img"  class="tool-thumbnail" v-for="img of tool.images" />
                        </div>
                    </div>
                </template>
                <div class="row mt-3">
                    <div class="tool-name col">
                        <h1>{{ tool.nicename }}</h1>
                    </div>
                </div>
                <div class="row mt-2">
                    <div class="col tool-short-descr">
                        <h5 class="m-0">{{ tool.short_descr }}</h5>
                    </div>
                </div>
                <div class="row mt-2">
                    <div class="tool-tags col">
                        <template v-for="tag,index in tool.tags">
                            <nuxt-link :to="'/tag/'+tag" class="tag-link highlight1" @click.native.prevent="resetSearch()">#{{ tag }}</nuxt-link>
                            &nbsp;
                        </template>
                    </div>
                </div>
                <div class="row mt-2">
                    <div class="col">
                        <template v-for="link,index in tool.links">
                            <a :href="link" class="text-custom2" target="_blank">{{ link }}</a><br />
                        </template>
                    </div>
                </div>
                <!-- <div class="row mt-2">
                    <div class="col">
                        <a :href="tool.homepage" class="tool-link text-custom2" target="_blank">{{ tool.homepage }}</a>
                        <template v-if="tool.extra_link">
                            <br /><a :href="tool.extra_link" class="text-custom2" target="_blank">{{ tool.extra_link }}</a>
                        </template>
                    </div>
                </div> -->
                <div class="row mt-4" v-if="tool.descr">
                    <div class="tool-descr col">
                        <p v-html="tool.descr.replace(/(?:\r\n|\r|\n)/g, '<br />')"></p>
                    </div>
                </div>
            </div>
            <div class="col col-1 d-none d-xl-block"></div>
            <div class="col col-2 d-none d-xl-block">
                <ToolContextualisation :tool="tool"></ToolContextualisation>
            </div>
        </div>
        <div class="row m-0 p-0 d-block d-xl-none">
            <div class="col m-0 p-0">
                <ToolContextualisation :tool="tool"></ToolContextualisation>
            </div>
        </div>
    </div>
</template>

<script>
import Ribbon from '~/components/Ribbon.vue';
import Sharer from '~/components/Sharer.vue';
import ToolContextualisation from '~/components/ToolContextualisation.vue';

export default {
    name: 'ToolDetails',
    props: [ 'tool' ],
    components: {
        ToolContextualisation, Sharer
    },
    methods: {
        setToolImage(i) {
            // alert(this.$config.ASSETS_URL+'/tools/'+i);
            document.getElementById('tool-picture').src = this.$config.ASSETS_URL+'/tools/'+i;

        },
        resetSearch: function () {
            // this.$store.commit( 'resetSearch', 1 );
            // this.$router.push( '/' );
        },
        // rate( rate_value ) {
        //     this.$store.dispatch( 'rate', [this.tool.id,rate_value] );
        // },
    },
    mounted() {
        this.$store.dispatch( 'createToolContextualisation', [5,this.tool.tags,this.tool.slug] );
    }
}
</script>

<style scoped>
#tool-details .link-close svg {
    font-size: 1.5em !important;
}
#tool-details .link-close:hover {
    color: #ddd;
}
#tool-details .tool-link:hover {
    text-decoration: underline;
}

#tool-details #tool-picture {
    aspect-ratio: 16 / 9;
    border-radius: 0.4em;
    /* height: 25em; */
    width: 100%;
}
/* @media all and (max-width: 572px) {
    #tool-details #tool-picture {
        height: 15em;
    }
}
@media all and (max-width: 768px) {
    #tool-details #tool-picture {
        height: 20em;
    }
} */
#tool-details .tool-thumbnail-container {
    /* display: block;
    float: left; */
    width: 120px !important;
}
#tool-details .tool-thumbnail {
    border-radius: 0.4em;
    height: 75px;
    margin-bottom: 1em;
    margin-right: 1.2em;
    width: 100px;
}
</style>