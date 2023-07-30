<script setup>
import json from "@/assets/wine.json"
import { onMounted, ref, watch } from "vue"

const serverUrl = "http://localhost:3000"
const imagesDir = "/src/assets/images/"

const item = ref("")
const itemList = ref([])
const idx = ref(0)

const create = () => {
  console.log(import.meta.url);
  item.value["ボトル画像"] = new URL(item.value["ボトル画像"], serverUrl + imagesDir).href
  item.value["商品紹介"]["サムネイル"] = new URL(item.value["商品紹介"]["サムネイル"], serverUrl + imagesDir).href
  item.value["商品紹介"]["紹介文"] = item.value["商品紹介"]["紹介文"].map(obj => {
    obj["画像"] = obj["画像"] ? new URL(obj["画像"], serverUrl + imagesDir).href : "";
    return obj
  })
  console.log(item.value);
}


onMounted(() => {
  itemList.value = json.map(j => j["名前"])
  item.value = json[idx.value]
  create()
})

watch(idx, () => {
  item.value = json[idx.value]
  create()
})



</script>

<template>
  <v-container class="fill-height">
    <v-responsive v-if="item" class="align-center text-center fill-height">
      <v-row>
        <v-col>
          <v-chip @click="idx = i" v-for="(name, i) in itemList" :key="name">
            {{ name }}
          </v-chip>
        </v-col>
      </v-row>
      <v-row class="bg-red">

        <v-col cols="12" md="4">
          <v-img height="300" :src="item['ボトル画像']" />
        </v-col>

        <v-col cols="12" md="8">
          <v-col cols="12" class="text-left">
            <h2>{{ item["名前"] }}</h2>
            <small>{{ item["英語名"] }}</small><br>
            <small>{{ item["容量"] }}ml</small>
            <p>
              {{ item["見出し"] }}
            </p>
            <v-divider></v-divider>
            <h3>
              {{ item["税抜"].toLocaleString() }}円(税込価格{{ Math.floor(item["税抜"] * 1.1) }}円)
            </h3>
          </v-col>
  
          <v-col cols="12">
            <v-list class="d-flex">
              <v-list-item v-for="link in item['リンク']" :key="link">{{ link }}</v-list-item>
            </v-list>
          </v-col>

        </v-col>
      
      </v-row>
      
      <v-row class="bg-blue">
          
        <v-col cols="12" md="5">
          <v-col cols="12">
            <p>
              ※画像はイメージのため、実際の商品と若干異なる場合がございます。
            </p>
          </v-col>
  
          <v-col cols="12">
            <h3>ボディ</h3>
            <v-col cols="8" class="mx-auto">
              <v-progress-linear
               :model-value="item['ボディ']"
               max="10"
               height="15"
               bg-color="pink-lighten-3"
               color="pink-lighten-1"
               >
             </v-progress-linear>
             <div class="d-flex justify-space-between">
               <span>ライト</span>
               <span>ミディアム</span>
               <span>フル</span>
             </div>
            </v-col>
          </v-col>
  
          <v-col cols="12">
            <h3>味わい</h3>
            <v-col cols="8" sm="6" class="mx-auto">
              <v-progress-circular
              :rotate="360"
              :size="150"
              :width="30"
              :model-value="15"
              color="teal"
              >
              </v-progress-circular>
              <div class="d-flex justify-space-between">
                <span>コク</span>
                <span>果実味</span>
                <span>酸味</span>
              </div>
            </v-col>
            <p>
              {{ item["テイスティングコメント"] }}
            </p>
          </v-col>
        </v-col>

        <v-col cols="12" md="7">
          <v-col cols="12">
            <p>
              {{ item["商品コメント"] }}
            </p>
          </v-col>
  
          <v-col cols="12">
            <h3>商品情報</h3>
            <v-table>
              <tbody>
                <tr
                  v-for="[k, v] in Object.entries(item['商品データ'])"
                  :key="k"
                >
                  <td>{{ k }}</td>
                  <td>{{ v }}</td>
                </tr>
              </tbody>
            </v-table>
          </v-col>
        </v-col>

      </v-row>

      <v-row class="bg-yellow">
        <v-col cols="12">
          <h2>商品紹介</h2>
          <v-img height="300" :src="item['商品紹介']['サムネイル']" />
          <p>{{ item['商品紹介']['冒頭'] }}</p>
          <template v-for="obj in item['商品紹介']['紹介文']" :key="obj">
            <div v-if="obj['画像']">
              <v-img height="300" :src="obj['画像']" />
            </div>
            <div v-if="obj['文章']">
              <p>
                {{ obj["文章"] }}
              </p>
            </div>
          </template>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>
