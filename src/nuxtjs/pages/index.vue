<template>
  <div class="home">
    <h1 class="home__title">
    出席調查問卷
    </h1>
    <div>Your token: {{ userToken }}</div>
    <form>
      <label for="isParticipate">是否出席：</label><br>
      <input type="radio" name="isParticipate" value="true" v-model="user.isParticipate">
      <input type="radio" name="isParticipate" value="false" v-model="user.isParticipate"><br>
      <label for="name">中文姓名：</label><br>
      <input type="text" id="name" name="name" placeholder="王聖方" v-model="user.name"><br>
      <input type="button" value="送出" @click="submit">
    </form>
  </div>
</template>

<style>
html,
body {
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
  Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  margin: 0;
  padding: 0;
}

.home {
  padding: 5rem 2rem;
  text-align: center;
}

.home__title {
  font-size: 6rem;
  line-height: 1.15;
}

.home__title__link {
  color: #06c755;
  text-decoration: none;
}

.home__title__link:hover {
  text-decoration: underline;
}

.home__badges {
  align-items: center;
  justify-content: center;
  display: flex;
  flex-wrap: nowrap;
  overflow: hidden;
  margin-bottom: 6rem;
}

.home__badges__badge:first-child {
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
}

.home__badges__badge:last-child {
  border-top-right-radius: 2px;
  border-bottom-right-radius: 2px;
}

.home__badges__badge {
  display: inline-block;
  padding: 0.3em 0.4em;
  font-size: 0.75rem;
  font-weight: 700;
  line-height: 1;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
}

.badge--primary {
  color: #353a40;
  border: 1px solid #353a40;
  background-color: transparent;
  padding-top: calc(0.3em - 1px);
  padding-bottom: calc(0.3em - 1px);
}

.badge--secondary {
  color: #fff;
  background-color: #353a40;
}

.home__buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.home__buttons__button {
  min-width: 250px;
  cursor: pointer;
  display: inline-block;
  font-weight: 400;
  text-align: center;
  vertical-align: middle;
  user-select: none;
  border: 1px solid transparent;
  padding: 0.375rem 0.75rem;
  font-size: 0.9375rem;
  line-height: 1.5;
  border-radius: 2px;
  text-decoration: none;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
  border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.button--primary {
  color: #fff;
  background-color: #00b900;
  border-color: #00b900;
}

.button--primary:hover {
  color: #fff;
  background-color: #009300;
  border-color: #008600;
}

.button--secondary {
  color: #fff;
  background-color: #353a40;
  border-color: #353a40;
}

.button--secondary:hover {
  color: #fff;
  background-color: #24272b;
  border-color: #1e2124;
}

.button--tertiary {
  background-color: transparent;
  background-image: none;
  color: #353a40;
  border-color: #353a40;
}

.button--tertiary:hover {
  color: #353a40;
  background-color: rgba(53, 58, 64, 0.1);
  border-color: #353a40;
}

@media screen and (max-width: 600px) {
  html {
    font-size: 12px;
  }

  .home__title {
    font-size: 4rem;
    line-height: 1.15;
  }

  .home__buttons__button {
    font-size: 1.5rem;
  }
}

@media screen and (max-width: 930px) {
  .home__buttons {
    flex-direction: column;
  }
}
</style>

<script>
import packageJson from "../package.json";
import axios from "axios";

export default {
  data: function () {
    return {
      version: packageJson.version,
      sdkVersion: "",
      liffError: "",
      userToken: "",
      user: {
        isParticipate: false,
        name: ""
      }
    };
  },
  methods: {
    submit: async function () {
      await axios.post(
          "https://ca1a-114-36-17-28.jp.ngrok.io/form",
          this.user,
          {
            headers: {
              Authorization: `Bearer ${this.userToken}`,
            },
          }
      );
      alert("送出成功");
    }
  },
  async mounted() {
    // mounted() is rendered when DOM is rendered
    // wait liff.init()
    this.$liffInit
        .then(() => {
          this.sdkVersion = liff.getVersion();
          this.userToken = liff.getAccessToken();
        })
        .catch((error) => {
          this.liffError = error;
        });
  }
};
</script>
