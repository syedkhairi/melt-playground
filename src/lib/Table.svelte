<script lang="ts">
    import { createTable, Subscribe, Render, createRender } from 'svelte-headless-table';
    import { addSortBy, addPagination } from 'svelte-headless-table/plugins';
    import Name from './Name.svelte';
	import { readable } from 'svelte/store';

    const data = readable([
        { name: 'Ada Lovelace', age: 21 },
        { name: 'Barbara Liskov', age: 52 },
        { name: 'Richard Hamming', age: 38 },
        { name: 'John Backus', age: 27 },
        { name: 'Grace Hopper', age: 34 },
        { name: 'Jack K', age: 21 },
        { name: 'Albert Einstein', age: 76 },
        { name: 'Marie Curie', age: 66 },
        { name: 'Nikola Tesla', age: 86 },
        { name: 'Alan Turing', age: 41 },
        { name: 'Ada Yonath', age: 81 },
        { name: 'Katherine Johnson', age: 101 },
        { name: 'Linus Torvalds', age: 51 },
        { name: 'Margaret Hamilton', age: 84 },
        { name: 'Tim Berners-Lee', age: 66 },
        { name: 'Steve Jobs', age: 56 },
        { name: 'Bill Gates', age: 65 },
        { name: 'Mark Zuckerberg', age: 37 },
        { name: 'Elon Musk', age: 50 },
        { name: 'Jeff Bezos', age: 57 },
        { name: 'Satya Nadella', age: 54 },
        { name: 'Sundar Pichai', age: 49 },
        { name: 'Larry Page', age: 48 },
        { name: 'Sergey Brin', age: 48 },
        { name: 'Susan Wojcicki', age: 53 },
        { name: 'Sheryl Sandberg', age: 52 },
        { name: 'Indra Nooyi', age: 65 },
        { name: 'Mary Barra', age: 60 },
    ]);

    const table = createTable(data, {
        sort: addSortBy(),
        page: addPagination({
            initialPageSize: 20
        })
    });

    const columns = table.createColumns([
        table.column({
            header: 'Name',
            accessor: 'name',
            cell: ({ row, value }) => {
                return createRender(Name, {
                    name: value,
                });
            },
            
        }),
        table.column({
            header: 'Age',
            accessor: 'age'
        }),
    ])

    const { headerRows, pageRows, tableAttrs, tableBodyAttrs, pluginStates } = table.createViewModel(columns);
    const { pageIndex, pageCount, pageSize, hasNextPage, hasPreviousPage } = pluginStates.page;
</script>

<table {...$tableAttrs}>
    <thead>
      {#each $headerRows as headerRow (headerRow.id)}
        <Subscribe rowAttrs={headerRow.attrs()} let:rowAttrs>
            {#each headerRow.cells as cell (cell.id)}
                <Subscribe attrs={cell.attrs()} let:attrs props={cell.props()} let:props>
                    <th {...attrs} on:click={props.sort.toggle}>
                        <Render of={cell.render()} />
                        {#if props.sort.order === 'asc'}
                        ⬇️
                        {:else if props.sort.order === 'desc'}
                        ⬆️
                        {/if}
                    </th>
                </Subscribe>
            {/each}
        </Subscribe>
      {/each}
    </thead>
    <tbody {...$tableBodyAttrs}>
      {#each $pageRows as row (row.id)}
        <Subscribe rowAttrs={row.attrs()} let:rowAttrs>
          <tr class="@container/row" {...rowAttrs}>
            {#each row.cells as cell (cell.id)}
              <Subscribe attrs={cell.attrs()} let:attrs>
                <!-- Using named container queries below -->
                <td {...attrs}>
                  <Render of={cell.render()} />
                </td>
              </Subscribe>
            {/each}
          </tr>
        </Subscribe>
      {/each}
    </tbody>
</table>

<div>
    <button
      on:click={() => $pageIndex--}
      disabled={!$hasPreviousPage}>Previous page</button
    >
    {$pageIndex + 1} out of {$pageCount}
    <button
      on:click={() => $pageIndex++}
      disabled={!$hasNextPage}>Next page</button
    >
</div>