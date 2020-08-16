<template>
    <div>
        <b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#">Question & Answer</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item href="#" disabled> {{ correctCount }}/{{ totalCount }}</b-nav-item>
      </b-navbar-nav>

      
    </b-collapse>
  </b-navbar>
    </div>
</template>
<script>
export default {
    props:{
        correctCount: Number,
        totalCount: Number
    }
}
</script>