<script lang="ts">

    import {Popover, PopoverContent, Trigger} from "$lib/components/ui/popover";
    import {Button} from "$lib/components/ui/button";
    import {ChevronsUpDown, Check} from "@lucide/svelte";
    import {Command, CommandItem, CommandList} from "$lib/components/ui/command";
    import {CommandEmpty, CommandGroup, CommandInput} from "$lib/components/ui/command/index.js";
    import {tick} from "svelte";
    import {cn} from "$lib/utils.ts";
    import {getLatestTermId, type Terms} from "$lib/types/terms.ts";
    import {goto} from "$app/navigation";

    interface Props {
        selectedTerm: string | undefined;
        terms: Terms;
    }

    let {
        selectedTerm,
        terms,
    }: Props = $props();

    let open = $state(false)
    let triggerRef = $state<HTMLButtonElement>(null!);
    let latestTerm = $derived(getLatestTermId(terms));

    function closeAndFocusTrigger() {
        open = false;
        triggerRef.focus();
    }

</script>

<Popover>
    <Trigger bind:ref={triggerRef}>
        {#snippet child({ props })}
            <Button
                    variant="outline"
                    class="w-[200px] justify-between"
                    {...props}
                    role="combobox"
                    aria-expanded={open}
            >
                {selectedTerm ? terms[selectedTerm] : "Select a term..."}
                <ChevronsUpDown class="ml-2 size-4 shrink-0 opacity-50" />
            </Button>
        {/snippet}
    </Trigger>
    <PopoverContent class="w-[200px] p-0">
        <Command>
            <CommandInput placeholder="Search term..."/>
            <CommandList>
                <CommandEmpty>No term found.</CommandEmpty>
                <CommandGroup>
                    {#each Object.entries(terms).toReversed() as [termId, termName]}
                        <CommandItem
                                value={termName}
                                onSelect={() => {
                                    goto(`?term=${termId}`);
                                    closeAndFocusTrigger();
                                }}
                        >
                            <Check
                                    class={cn(selectedTerm !== termId && "text-transparent")}
                            />
                            {#if termId === latestTerm}
                                Latest ({termName})
                            {:else}
                                {termName}
                            {/if}
                        </CommandItem>
                    {/each}
                </CommandGroup>
            </CommandList>
        </Command>
    </PopoverContent>
</Popover>