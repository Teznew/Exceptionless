<script lang="ts">
	import IconChevronRight from '~icons/mdi/chevron-right';
	import type { StackSummaryModel, SummaryModel, SummaryTemplateKeys } from '$lib/models/api';

	export let badgeClass: string;
	export let showBadge: boolean;
	export let summary: SummaryModel<SummaryTemplateKeys>;
	const source = summary as StackSummaryModel<'stack-error-summary'>;
</script>

<div class="line-clamp-2">
	{#if showBadge}
		<span class="label label-default {badgeClass}">
			{source.status}
		</span>
	{/if}

	<strong>
		<abbr title={source.data.TypeFullName}>{source.data.Type}</abbr>
		{#if !source.data.Method}
			:
		{/if}
	</strong>

	{#if source.data.Method}
		in
		<strong>
			<abbr title={source.data.MethodFullName}>{source.data.Method}</abbr>
		</strong>
	{/if}

	<a href="/stack/{source.id}" class="inline">
		{source.title}
	</a>
</div>

{#if source.data.Path}
	<div class="hidden sm:block text-gray-500 ml-6 text-sm">
		<IconChevronRight class="inline" />
		<span class="inline line-clamp-1">{source.data.Path}</span>
	</div>
{/if}
