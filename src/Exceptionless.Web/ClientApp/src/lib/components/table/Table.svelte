<script lang="ts">
	import KeyboardArrowDownIcon from '~icons/mdi/keyboard-arrow-down';
	import KeyboardArrowUpIcon from '~icons/mdi/keyboard-arrow-up';

	import { flexRender, type Header, type Table as TableType } from '@tanstack/svelte-table';
	import { createEventDispatcher } from 'svelte';
	import type { Readable } from 'svelte/store';

	type TData = $$Generic;
	export let table: Readable<TableType<TData>>;

	const dispatch = createEventDispatcher();

	function getHeaderColumnClass(header: Header<TData, unknown>) {
		return `${
			(header.column.columnDef.meta as { class?: string })?.class || ''
		} border border-base-300`.trimStart();
	}
</script>

<slot name="header" {table} />

<table class="table table-zebra table-xs border border-base-300">
	<thead>
		{#each $table.getHeaderGroups() as headerGroup}
			<tr>
				{#each headerGroup.headers as header}
					<th class={getHeaderColumnClass(header)}>
						{#if !header.isPlaceholder}
							<button
								on:click={header.column.getToggleSortingHandler()}
								disabled={!header.column.getCanSort()}
								class="flex items-center"
							>
								<svelte:component
									this={flexRender(
										header.column.columnDef.header,
										header.getContext()
									)}
								/>
								{#if header.column.getIsSorted() === 'asc'}
									<KeyboardArrowUpIcon />
								{:else if header.column.getIsSorted() === 'desc'}
									<KeyboardArrowDownIcon />
								{/if}
							</button>
						{/if}
					</th>
				{/each}
			</tr>
		{/each}
	</thead>
	<tbody>
		<tr class="hidden only:table-row text-center">
			<td class="border border-base-300" colspan={$table.getVisibleLeafColumns().length}>
				No data was found with the current filter.
			</td>
		</tr>
		{#each $table.getRowModel().rows as row}
			<tr
				class="hover cursor-pointer"
				on:click|preventDefault={() => dispatch('rowclick', row.original)}
			>
				{#each row.getVisibleCells() as cell}
					<td class="border border-base-300">
						<svelte:component
							this={flexRender(cell.column.columnDef.cell, cell.getContext())}
						/>
					</td>
				{/each}
			</tr>
		{/each}
	</tbody>
</table>

<slot name="footer" {table} />
