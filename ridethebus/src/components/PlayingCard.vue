<template>
    <div>
        <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Card_back_06.svg/209px-Card_back_06.svg.png" />
    </div>
</template>

<script>
export default {
    props: {
        image: String
    },
    data: function() {
        return {
            imgsrc: this.image
        }
    }
}
</script>