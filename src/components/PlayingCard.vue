<template>
    <div>
        <img :src="imgsrc" />
    </div>
</template>

<script>
export default {
    props: {
        imgsrc: {
            type: String,
            default() {
                return 'https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Card_back_06.svg/209px-Card_back_06.svg.png'
            }
        }
    },
    data: function() {
        return {
        }
    }
}
</script>