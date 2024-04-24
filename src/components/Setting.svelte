<script>
    export let mode = "list";
    export let settings = [];
</script>

<div class="card w-full bg-base-200 shadow">
    <div class="card-body">
        <h2 class="card-title">系统设定</h2>
        {#if mode === "list"}
            <div class="flex flex-row mb-2">
                <button
                    class="btn btn-secondary btn-sm"
                    on:click={() => {
                        mode = "create";
                    }}>新增</button
                >
            </div>
            <div class="overflow-x-auto">
                <table class="table table-xs table-pin-rows table-pin-cols">
                    <thead>
                        <tr>
                            <th></th>
                            <td>ID</td>
                            <td>ROOT ID</td>
                            <td>PARENT ID</td>
                            <td>TAGS</td>
                            <td>DETAIL</td>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each settings as setting, index}
                            <tr>
                                <th>{index + 1}</th>
                                <td>{setting.id}</td>
                                <td>{setting.rootId}</td>
                                <td>{setting.parentId}</td>
                                <td>{setting.tags}</td>
                                <td>{setting.detail}</td>
                                <th class="text-center">
                                    <button
                                        class="btn btn-info btn-outline btn-xs"
                                        >编辑</button
                                    >
                                </th>
                            </tr>
                        {/each}
                    </tbody>
                    <tfoot>
                        <tr>
                            <th></th>
                            <td>ID</td>
                            <td>ROOT ID</td>
                            <td>PARENT ID</td>
                            <td>TAGS</td>
                            <td>DETAIL</td>
                            <th></th>
                        </tr>
                    </tfoot>
                </table>
            </div>
        {:else if mode === "create" || mode === "update"}
            <button
                class="btn btn-secondary btn-sm"
                on:click={() => {
                    mode = "list";
                }}>返回</button
            >
            <form>
                <div class="card-actions justify-end">
                    <button class="btn btn-primary">Buy Now</button>
                </div>
            </form>
        {/if}
    </div>
</div>
