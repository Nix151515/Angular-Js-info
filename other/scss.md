

 - Variables, functions:
$containerWidth: 100px;
$cardHeight: 60px;
$itemIdClass: '.itemClass'; // string variable

@function doubleWidth($width) {
  @return $width * 2;
}

#{$itemIdClass} {
    width: doubleWidth(5)
}



- Mixins 
@mixin is used to define a mixin in SCSS to reuse styles.
@include is used to include a mixin in the current selector.

@mixin showProgress($progress: 0) {
    @if ($progress > 80) {
        background-color: green
    }
    color: #fff
}

.progress-bar {
    @include(showProgress(90))

    &__loading {     // nesting -> .progress-bar__loading
        color: blue
    }
}

.progress-bar2 {
    @extend .progress-bar;
    margin-top: 2px;
}



- @else if, @while, @for -> possible