<script>
    import axios from "axios";
    import { CRATE_API_HOST } from "../constants.mjs";

    export let mode = "list";
    export let settings = [];

    let id = "";
    let k = "";
    let v = "";

    async function submit() {
        if (mode === "update") {
            const s = settings.find((it) => it._id === id);
            if (!s) return;
            const uuid = JSON.parse(s.state)?.uuid;
            const response = await axios.put(
                CRATE_API_HOST + `/crate/setting/${uuid}/${id}`,
                {
                    detail: JSON.stringify({ name: k, value: v }),
                },
            );
            if (response.status >= 400) {
                console.error(response);
                return;
            }
            location.refresh(true);
            mode = "list";
        }
    }
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
                                        on:click={() => {
                                            id = setting._id;
                                            k =
                                                JSON.parse(setting?.detail)
                                                    ?.name || "";
                                            v =
                                                JSON.parse(setting?.detail)
                                                    ?.value || "";
                                            mode = "update";
                                        }}>编辑</button
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
            <div class="flex flex-row"></div>
            <form on:submit|preventDefault={submit} class="flex flex-col gap-4">
                <label class="input input-bordered flex items-center gap-2">
                    名称
                    <input type="text" bind:value={k} class="grow" />
                </label>
                <label class="input input-bordered flex items-center gap-2">
                    值
                    <input type="text" bind:value={v} class="grow" />
                </label>
                <div class="card-actions justify-between">
                    <button
                        class="btn btn-secondary btn-sm"
                        on:click={() => (mode = "list")}>返回</button
                    >
                    <button type="submit" class="btn btn-primary btn-sm"
                        >提交</button
                    >
                </div>
            </form>
        {/if}
    </div>
</div>
