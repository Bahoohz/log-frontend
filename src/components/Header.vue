<template>
  <div>
    <b-img :src="require('../assets/Header.jpg')" style="width:100%;"></b-img>
    <b-navbar toggleable="lg" type="dark" variant="dark" style="width:15%">
      <b-navbar-brand style="margon-left: 100px;">เมนูหลัก</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav class="ml-auto"></b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>
