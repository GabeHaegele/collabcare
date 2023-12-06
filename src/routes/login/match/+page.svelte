<script lang="ts">
    import AuthCheck from "$lib/components/AuthCheck.svelte";
    import { db, user, userData } from "$lib/firebase";
    import { doc, getDoc, writeBatch } from "firebase/firestore";

    async function findMatches() {
        let ref;
        console.log("finding matches");
        if("APP"=="APP") {
            ref = doc(db, "role", "physician");
        }else{
            ref = doc(db, "role", "app");
        }
        
        const docSnap = await getDoc(ref);
        console.log("DOC DATA", docSnap.data())
    }
    findMatches();
</script>

<AuthCheck>
    <h2>Match</h2>

</AuthCheck>