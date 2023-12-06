<script lang="ts">
    import AuthCheck from "$lib/components/AuthCheck.svelte";
    import { db, user, userData, writableArray } from "$lib/firebase";
    import { QuerySnapshot, collection, doc, getDoc, getDocs, query, where, writeBatch } from "firebase/firestore";

    async function findMatches() {
        let roles = ['app', 'physician'];
        let q;
        $writableArray = [];

        console.log("finding matches");
        console.log("userData.role = ", $userData.role);

        if ($userData.role == roles[0]) {
            q = query(collection(db, "users"), where("role", "==", roles[1]));
        } else {
            q = query(collection(db, "users"), where("role", "==", roles[0]));
        } 
        
        const querySnapshot = await getDocs(q);

        console.log("querySnapshot: ", querySnapshot.docs);

        querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            console.log(doc.id, " => ", doc.data());
            $writableArray = [...$writableArray, {
                uid: doc.id,
                name: doc.get("displayName")
            }];
            console.log(doc.id, " => ", doc.get("displayName"));

        });
        $writableArray = $writableArray.slice(0, $writableArray.length);
        console.log("Match: ", $writableArray);
    }
    findMatches();
</script>

<AuthCheck>
    <h2>Matches:</h2>
    <ul>
        {#each $writableArray as id, i}
            <li>
                <p>{i+1}: {$writableArray[i].name}</p>
            </li>
        {/each}
    </ul>
    
</AuthCheck>