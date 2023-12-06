<script lang="ts">
    import AuthCheck from "$lib/components/AuthCheck.svelte";
    import { db, user, userData } from "$lib/firebase";
    import { doc, getDoc, writeBatch, deleteField } from "firebase/firestore";
    

    let roles = ['app', 'physician']
    let role = "";
    let patients = ['Children', 'Adults', 'Elderly', 'Complex'];
    let selectedPatients = [];
    let proximity = "";
    let procedure = "";
    let learning = "";
    let coverage = "";
    let displayName = "";

    async function confirmRole() {
        console.log("whatdafuac", role);
        const batch = writeBatch(db);
        batch.set(doc(db, "role", role), { uid: $user?.uid });
        if (role == roles[0]) {
            batch.update(doc(db, "role", roles[1]), { uid: deleteField() });
        } else {
            batch.update(doc(db, "role", roles[0]), { uid: deleteField() });
        }
        
        batch.set(doc(db, "users", $user!.uid), {
            role,
            proximity,
            procedure,
            coverage,
            learning,
            displayName,
        });
        await batch.commit();
    }

    async function confirmProximity() {
        console.log("proximity", proximity);
        const batch = writeBatch(db);
        batch.update(doc(db, "users", $user!.uid), {
            proximity,
        });

        await batch.commit();
    }

    async function confirmProcedure() {
        console.log("procedure", procedure);
        const batch = writeBatch(db);
        batch.update(doc(db, "users", $user!.uid), {
            procedure,
        });

        await batch.commit();
    }

    async function confirmCoverage() {
        console.log("coverage", coverage);
        const batch = writeBatch(db);
        batch.update(doc(db, "users", $user!.uid), {
            coverage,
        });

        await batch.commit();
    }

    async function confirmLearning() {
        console.log("learning", learning);
        const batch = writeBatch(db);
        batch.update(doc(db, "users", $user!.uid), {
            learning,
        });

        await batch.commit();
    }
</script>

<AuthCheck>
    <h2>I am...</h2>
    <div class="flex space-x-6">
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmRole} bind:group={role} value="physician" id="bordered-radio-1" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-1" class="w-full py-4 ml-2 text-sm font-medium">a Physician</label>
        </div>
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmRole} bind:group={role} value="app" id="bordered-radio-2" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-2" class="w-full py-4 ml-2 text-sm font-medium">an APP</label>
        </div>
    </div>
</AuthCheck>

{#if role == "physician"}
    <h1 class="pt-20 pb-10 font-semibold text-xl">Legal Scope of Work</h1>

    <section class="pb-10">
        <h2 class="font-semibold space-y-10">I need an APP that can see these types of patients:</h2>
        {#each patients as patient}
            <label>
                <input type="checkbox" value={patient} bind:group={selectedPatients} />
                {patient}
            </label>
        {/each}
    </section>

    <h1 class="pt-20 pb-10 font-semibold text-xl">Independence</h1>

    <h2>I would prefer an APP...</h2>
    <div class="flex space-x-6">
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmProximity} bind:group={proximity} value="close" id="proximity-radio-1" type="radio" name="proximity-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="proximity-radio-1" class="w-full py-4 ml-2 text-sm font-medium">in close proximity</label>
        </div>
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmProximity} bind:group={proximity} value="far" id="proximity-radio-2" type="radio" name="proximity-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="proximity-radio-2" class="w-full py-4 ml-2 text-sm font-medium">in a separate location</label>
        </div>
    </div>

    <h1 class="pt-20 pb-10 font-semibold text-xl">Procedures</h1>
    
    <h2>I am...</h2>
    <div class="flex space-x-6">
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmProcedure} bind:group={procedure} value="willing" id="procedure-radio-1" type="radio" name="procedure-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="procedure-radio-1" class="w-full py-4 ml-2 text-sm font-medium">willing to do procedures</label>
        </div>
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmProcedure} bind:group={procedure} value="unwilling" id="procedure-radio-2" type="radio" name="procedure-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="procedure-radio-2" class="w-full py-4 ml-2 text-sm font-medium">not willing to do procedures</label>
        </div>
    </div>

    <h1 class="pt-20 pb-10 font-semibold text-xl">Learning Opportunities</h1>

    <h2>I am...</h2>
<div class="flex space-x-6">
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmLearning} bind:group={learning} value="open" id="learning-radio-1" type="radio" name="learning-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="learning-radio-1" class="w-full py-4 ml-2 text-sm font-medium">open to learning opportunities</label>
    </div>
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmLearning} bind:group={learning} value="not open" id="learning-radio-2" type="radio" name="learning-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="learning-radio-2" class="w-full py-4 ml-2 text-sm font-medium">not open to learning opportunities</label>
    </div>
</div>

    <h1 class="pt-20 pb-10 font-semibold text-xl">On-Call Coverage</h1>

    <h2>I am...</h2>
<div class="flex space-x-6">
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmCoverage} bind:group={coverage} value="available" id="coverage-radio-1" type="radio" name="coverage-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="coverage-radio-1" class="w-full py-4 ml-2 text-sm font-medium">available for on-call coverage</label>
    </div>
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmCoverage} bind:group={coverage} value="unavailable" id="coverage-radio-2" type="radio" name="coverage-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="coverage-radio-2" class="w-full py-4 ml-2 text-sm font-medium">unavailable for on-call coverage</label>
    </div>
</div>
{/if}

{#if role == "app"}
    <h1 class="pt-20 pb-10 font-semibold text-xl">Legal Scope of Work</h1>

    <section>
        <h2>I am trained to see these types of patients:</h2>
        {#each patients as patient}
            <label>
                <input type="checkbox" value={patient} bind:group={selectedPatients} />
                {patient}
            </label>
        {/each}
    </section>

    <h1 class="pt-20 pb-10 font-semibold text-xl">Independence</h1>

    <h2>I would prefer a Physician...</h2>
    <div class="flex space-x-6">
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmProximity} bind:group={proximity} value="close" id="bordered-radio-1" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-1" class="w-full py-4 ml-2 text-sm font-medium">in close proximity</label>
        </div>
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmProximity} bind:group={proximity} value="far" id="bordered-radio-2" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-2" class="w-full py-4 ml-2 text-sm font-medium">in a separate location</label>
        </div>
    </div>

    <h1 class="pt-20 pb-10 font-semibold text-xl">Procedures</h1>
    
    <h2>I am...</h2>
    <div class="flex space-x-6">
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmRole} bind:group={role} value="physician" id="bordered-radio-1" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-1" class="w-full py-4 ml-2 text-sm font-medium">willing to do procedures</label>
        </div>
        <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
            <input on:input={confirmRole} bind:group={role} value="app" id="bordered-radio-2" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
            <label for="bordered-radio-2" class="w-full py-4 ml-2 text-sm font-medium">not willing to do procedures</label>
        </div>
    </div>

    <h1 class="pt-20 pb-10 font-semibold text-xl">Learning Opportunities</h1>

    <h2>I am...</h2>
<div class="flex space-x-6">
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmRole} bind:group={role} value="physician" id="bordered-radio-1" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="bordered-radio-1" class="w-full py-4 ml-2 text-sm font-medium">open to learning opportunities</label>
    </div>
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmRole} bind:group={role} value="app" id="bordered-radio-2" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="bordered-radio-2" class="w-full py-4 ml-2 text-sm font-medium">not open to learning opportunities</label>
    </div>
</div>

    <h1 class="pt-20 pb-10 font-semibold text-xl">On-Call Coverage</h1>

    <h2>I am...</h2>
<div class="flex space-x-6">
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmRole} bind:group={role} value="physician" id="bordered-radio-1" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="bordered-radio-1" class="w-full py-4 ml-2 text-sm font-medium">available for on-call coverage</label>
    </div>
    <div class="flex items-center pl-4 pr-4 border border-gray-200 hover:rounded hover:bg-blue-500 dark:border-gray-700">
        <input on:input={confirmRole} bind:group={role} value="app" id="bordered-radio-2" type="radio" name="bordered-radio" class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
        <label for="bordered-radio-2" class="w-full py-4 ml-2 text-sm font-medium">available for on-call coverage</label>
    </div>
</div>
{/if}