<template lang='pug'>
  nav.menu
    input#menu-open.menu-open(type='checkbox' href='#' name='menu-open')
    label.menu-open-button(for='menu-open')
      .hamburger
        span
        span
        span
    system-button.menu-item(type='shutdown')
    system-button.menu-item(type='restart')
    system-button.menu-item(type='suspend')
    system-button.menu-item(type='settings')

    // filters
    svg(xmlns='http://www.w3.org/2000/svg' version='1.1')
      defs
        filter#shadowed-goo
          <feGaussianBlur in="SourceGraphic" result="blur" stdDeviation="10" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 18 -7" result="goo" />
          <feGaussianBlur in="goo" stdDeviation="3" result="shadow" />
          <feColorMatrix in="shadow" mode="matrix" values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 1 -0.2" result="shadow" />
          <feOffset in="shadow" dx="1" dy="1" result="shadow" />
          <feBlend in2="shadow" in="goo" result="goo" />
          <feBlend in2="goo" in="SourceGraphic" result="mix" />
        filter#goo
          <feGaussianBlur in="SourceGraphic" result="blur" stdDeviation="10" />
          <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 5 -7" result="goo" />
          <feBlend in2="goo" in="SourceGraphic" result="mix" />
</template>

<script>
  import SystemButton from '@/components/SystemButton';

  export default {
    name: 'settings-button',
    components: {
      SystemButton
    }
  }
</script>

<style lang='scss' scoped>
@import "mathsass/dist/_math.scss";
//vars
$pi:3.14;

//config
$menu-items:4;
$open-distance:110px;
$opening-angle:$pi - 1.6;

%ball{
  background:var(--color-active);
  border-radius:100%;
  $radius: 60px;
  width:$radius;
  height:$radius;
  position:absolute;
  color:white;
  text-align:center;
  line-height:$radius;
  transform:translate3d(0,0,0);
  transition:transform ease-out 200ms;
  cursor:pointer;
}
.menu-open{
  display:none;
}
.menu-item{
  @extend %ball;
}

.hamburger{
  $hamburger-spacing:8px;
  *{
    $width:25px;
    $height:3px;
    width:$width;
    height:$height;
    background:white;
    display:block;
    position:absolute;
    top:50%;
    left:50%;
    margin-left:-$width/2;
    margin-top:-$height/2;
    transition:transform 200ms;
  }
  :nth-child(1){
    transform:translate3d(0,-$hamburger-spacing,0);
  }
  :nth-child(2){
    transform:translate3d(0,0,0);
  }
  :nth-child(3){
    transform:translate3d(0,$hamburger-spacing,0);
  }
}

.menu-open:checked+.menu-open-button{
  .hamburger{
    :nth-child(1){
      transform:translate3d(0,0,0) rotate(45deg); 
    }
    :nth-child(2){
      transform:translate3d(0,0,0) scale(0.1,1);
    }
    :nth-child(3){
      transform:translate3d(0,0,0) rotate(-45deg); 
    }
  }
}
.menu{
  filter:url('#shadowed-goo');
}

.menu-item{
  &:hover{
    background:white;
    color:var(--color-active);
  }
  @for $i from 1 through $menu-items{
    &:nth-child(#{$i+2}){
      transition-duration:10ms+(30ms*($i));
    }
  }
}

.menu-open-button{
  @extend %ball;
  z-index:2;
  transition-timing-function:cubic-bezier(0.175, 0.885, 0.320, 1.275);
  transition-duration:400ms;
  transform:scale(1.1,1.1) translate3d(0,0,0);
  cursor:pointer;
}
.menu-open-button:hover{
  transform:scale(1.2,1.2) translate3d(0,0,0);
}
.menu-open:checked+.menu-open-button{
  transition-timing-function:linear;
  transition-duration:200ms;
  transform:scale(0.8,0.8) translate3d(0,0,0);
}

.menu-open:checked~.menu-item{
  transition-timing-function:cubic-bezier(0.935, 0.000, 0.340, 1.330);
  @for $i from 1 through $menu-items{
    $angle:(($pi - $opening-angle)/.51)+(($opening-angle/($menu-items - 1))*($i - 1));
    
    &:nth-child(#{$i+2}){
      transition-duration:80ms+(80ms*$i);
      transform:translate3d(cos($angle)*$open-distance, sin($angle)*$open-distance,0);
    }
  }
}
svg{
  width: 0;
  height: 0;
}
</style>
