<template>
  <div class="page">
    <div class="celebrity"
      v-if="subject.id">
      <div class="base">
        <div class="cover">
          <img :src="subject.avatars.medium | handleImageUrl"
            width="92"
            alt="">
        </div>
        <div class="r">
          <h3>{{subject.name}}</h3>
          <ul>
            <li v-if="subject.name_en">英文名：{{subject.name_en}}</li>
            <li v-if="subject.born_place">出生地：{{subject.born_place}}</li>
          </ul>
        </div>
      </div>
      <div class="detail">
        <h2>主要参演作品</h2>
        <div class="cont movies">
          <section class="ii return_name"
            v-for="item in subject.works"
            :key="item.id">
            <router-link :to="{ name: 'Detail', params: { id: item.subject.id }}">
              <img class="delay"
                :src="item.subject.images.small | handleImageUrl">
              <div class="r">
                <p>{{item.subject.title}}</p>
                <p class="gray">作品别名：{{item.subject.original_title}}</p>
                <p class="roles">剧中身份：{{item.roles.join('、')}}</p>
                <p class="rank"
                  v-run="register('rank')"
                  :data-average="item.subject.rating.average"></p>
              </div>
            </router-link>
          </section>
        </div>
      </div>
      <div class="aka"
        v-if="subject.aka.length">
        <h2>其他称呼</h2>
        <span v-for="item in subject.aka"
          :key="item">{{item}}</span>
      </div>
    </div>
    <loading />
  </div>
</template>
<script type="text/javascript">
import { mapGetters, mapActions } from 'vuex';
import Loading from '../common/Loading';

export default {
  name: 'celebrity',
  data() {
    return {
      elements: [],
    };
  },
  computed: {
    ...mapGetters({
      subject: 'celebrityData',
    }),
  },
  components: {
    Loading,
  },
  created() {
    this.getCelebrity();
  },
  directives: {
    run(el, binding) {
      if (typeof binding.value === 'function') { binding.value(el); }
    },
  },
  updated() {
    const starsEle = this.elements;
    starsEle.forEach((item) => {
      const num = Math.round(item.getAttribute('data-average'));
      if (num) {
        item.innerHTML = `<span class="smallstar${num} smallstar"></span>${num}分`;
      }
    });
  },
  methods: {
    ...mapActions([
      'getCelebrity',
    ]),
    register() {
      return (el) => {
        this.elements.push(el);
      };
    },
  },
};

</script>
