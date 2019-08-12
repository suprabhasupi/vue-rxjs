<template>
  <section>
  	{{ activeTab$ }}
    <b-tabs v-model="activeTab">
      <b-tab-item label="Luke"></b-tab-item>
      <b-tab-item label="Darth"></b-tab-item>
      <b-tab-item label="Leia"></b-tab-item>
    </b-tabs>
  </section>
</template>

<script>
// import { from, merge } from "rxjs";
import { pluck } from "rxjs/operators";

export default {
	data() {
    return {
      activeTab: 0
    };
  },

	domStreams:["click$", "imageError$"],
  subscriptions() {
  	const activeTab$ = this.$watchAsObservable("activeTab", {
      immediate: true
    }).pipe(pluck("newValue"));

    // const activeTab$ = this.$watchAsObservable("activeTab", {
    //   immediate: true
    // })

    return {
    	activeTab$
    }
  }
};
</script>

<!-- You most likely already have data or properties in your template which are controlled by third-party components or updated using data binding. You can still use this data as stream by leveraging vue-rx's $watchAsObservable then chaining RxJS operators onto it as a new stream. -->