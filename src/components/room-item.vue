<template>
  <fragment>
    <div class="md-list-item-text">
      <router-link tag="div" :to="linkTo">{{ data.name }}</router-link>
      <span class="creator-name">Created by {{ creatorFullName }}</span>
    </div>
    <div v-if="showActionButtons" class="action-buttons">
      <md-button class="md-icon-button md-list-action" @click="onEditClick(data)">
        <md-icon>edit</md-icon>
      </md-button>
      <md-button class="md-icon-button md-list-action" @click="onDeleteClick(data)">
        <md-icon>delete</md-icon>
      </md-button>
    </div>
  </fragment>
</template>

<script>
import firebase from 'firebase/app';
import { usersCollection } from '@/main.js';
export default {
  name: 'room-item',
  props: {
    data: {
      type: Object,
      required: true,
    },
    onEditClick: {
      type: Function,
      required: false,
      default: () => {},
    },
    onDeleteClick: {
      type: Function,
      required: false,
      default: () => {},
    },
  },
  data() {
    return {
      userId: firebase.auth().currentUser.uid,
      creator: null,
    };
  },
  firestore() {
    return {
      creator: usersCollection.doc(this.data.creatorId),
    };
  },
  computed: {
    linkTo() {
      return `/rooms/${this.data.id}`;
    },
    showActionButtons() {
      return this.data.creatorId === this.userId;
    },
    creatorFullName() {
      if (!this.creator) {
        return null;
      }
      return `${this.creator.firstName} ${this.creator.lastName}`;
    },
  },
};
</script>

<style scoped lang="scss">
.md-list-item {
  .action-buttons {
    display: none;
  }
  .creator-name {
    display: none;
  }
  .router-link-active {
    color: var(--md-theme-default-primary);
  }
  &:hover {
    .action-buttons {
      display: block;
    }
  }
  .md-list-item-text {
    cursor: pointer;
  }
}
</style>
