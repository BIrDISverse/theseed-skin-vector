<template>
    <form id="searchform" class="form-inline" v-on:submit.prevent>
        <div id="simpleSearch">
            <input name="search" :placeholder="$store.state.config['wiki.site_name'] + ' 검색'" :title="$store.state.config['wiki.site_name'] + '검색 [Alt+Shift+f]'" accesskey="f" id="searchInput" tabindex="1" autocomplete="off" type="search" v-on:input="searchText = $event.target.value" v-model="searchTextModel" @blur="blur" @focus="focus" @input="inputChange" @keydown.enter="keyEnter" @keydown.tab="keyEnter" @keydown.up="keyUp" @keydown.down="keyDown">
            <input @click="onClickSearchButton" name="go" value="보기" title="이 이름의 문서가 존재하면 그 문서로 바로 가기" id="searchButton" class="searchButton" type="submit">
            <div v-if="show" class="v-autocomplete-list">
                <div class="v-autocomplete-list-item" v-for="(item, i) in internalItems" @click="onClickItem(item)" v-bind:key="i" :class="{'v-autocomplete-item-active': i === cursor}" @mouseover="cursor = i">
                    <div>{{ item }}</div>
                </div>
            </div>
        </div>
    </form>
</template>

<script>
import AutocompleteMixin from '~/mixins/autocomplete';

export default {
    mixins: [AutocompleteMixin],
    methods: {
        onClickSearchButton() {
            if (!this.searchText) return;
            this.$router.push('/Go?q=' + encodeURIComponent(this.searchText));
        },
    },
    watch: {
        $route() {
            this.searchTextModel = '';
            this.searchText = '';
            this.cursor = -1;
            this.items = [];
        }
    }
}
</script>

<style scoped>
.v-autocomplete-list {
    position: absolute;
    z-index: 3;
    border: 1px solid #CCC;
    background-color: #fff;
    width: 10.8rem;
}

@media (max-width: 1023px) {
    .v-autocomplete-list {
        width: 100%;
    }
}

.theseed-dark-mode .v-autocomplete-list {
    background-color: #2d2f34;
    border: 1px solid #383b40;
}

.v-autocomplete-list-item {
    cursor: pointer;
    color: #373a3c;
    padding: 0.5rem;
    word-break: break-all;
}
.theseed-dark-mode .v-autocomplete-list-item {
    color: #ddd;
}
 .v-autocomplete-list-item.v-autocomplete-item-active {
    background-color: #f3f6fa;
}
.theseed-dark-mode .v-autocomplete-list-item.v-autocomplete-item-active {
    background-color: #383b40;
}
</style>