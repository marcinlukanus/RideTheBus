<template>
    <div>
        <b-button v-b-modal.modal-1 variant="secondary">Rules</b-button>

        <b-modal
            ok-only
            ok-variant="success"
            id="modal-1" 
            title="Rules"
        >
            <p><i>Note: You use a fresh deck everytime you restart. No counting cards!</i></p>
            <p>Each number represents the card you're picking. The number in the brackets signifies amount of time to drink for incorrect guess for that card.</p>
            <ol class="my-2">
                <li>Pick the color (<strong>RED</strong> or <strong>BLACK</strong>) [2]</li>
                <li>Pick the number (<strong>HIGHER</strong> or <strong>LOWER</strong> or <strong>SAME</strong>) [4]</li>
                <li>Pick the range (<strong>INSIDE</strong> or <strong>OUTSIDE</strong> or <strong>SAME</strong>) [6]</li>
                <li>Pick the suit (<strong>HEARTS</strong> or <strong>DIAMONDS</strong> or <strong>CLUBS</strong> or <strong>SPADES</strong>) [8]</li>
            </ol>
            <p>If you get a card wrong, click the Draw Cards button to restart.</p>
        </b-modal>
    </div>
</template>