<template>
  <div class="avatar-menu">
    <div class="user-avatar">
      <avatar username="Jane Doe" :inline="true" :size="100" :rounded="false"></avatar>
      <button class="upload-img">
        <i class="fas fa-camera"></i>
      </button>
    </div>

    <h4>Nextore admin</h4>
    <p>admin@nexify.com.hk</p>
    <button class="btn btn-outline-secondary">Configure your account</button>

    <hr />

    <ul class="account-list">
      <li>
        <div class="account-item">
          <div class="avatar">
            <avatar username="Tsz Lam" :inline="true" :size="25" :rounded="false"></avatar>
          </div>
          <div class="info">
            <div class="header">Tsz Lam</div>
            <div class="email">tlyau@nexify.com.hk</div>
          </div>
        </div>
      </li>
      <li>
        <div class="account-item">
          <div class="avatar">
            <i class="fas fa-user-plus"></i>
          </div>
          <div class="info">
            <div>Add another account</div>
          </div>
        </div>
      </li>
    </ul>

    <hr />

    <button class="btn btn-outline-secondary">Logout</button>

    <hr />

    <p>v{{packageJson.version}}</p>
  </div>
</template>

<script>
import Avatar from "vue-avatar";
import packageJson from "@/../package.json";

export default {
  components: {
    Avatar
  },
  data() {
    return {
      packageJson
    };
  }
};
</script>

<style scoped lang="scss">
@import "~bootstrap";
@import "~@fortawesome/fontawesome-free/css/all.css";

.avatar-menu {
  text-align: center;

  .user-avatar {
    margin: 10px auto;
    width: 100px;
    position: relative;

    .upload-img {
      border-radius: 50%;
      width: 25px;
      height: 25px;
      font-size: 15px;
      border: 0;
      box-shadow: 0px 1px 1px 1px rgba(0, 0, 0, 0.2);
      bottom: 0;
      right: 0;
      position: absolute;
    }
  }

  .account-list {
    text-align: left;
    list-style: none;
    padding: 0;

    .account-item {
      display: flex;
      align-items: center;
      padding: 10px 0;

      &:hover {
        background: $gray-300;
      }

      .avatar {
        margin: 0 10px;
      }

      .info {
        margin: 0;

        .header {
          font-weight: 500;
        }

        .email {
          font-weight: 200;
          color: $gray-600;
          font-size: 12px;
        }
      }
    }
  }
}
</style>