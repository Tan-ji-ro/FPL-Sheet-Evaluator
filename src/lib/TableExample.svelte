<script>
    import { Label, Table } from "sveltestrap";

    var example_data = [];

    example_data.push(
        {
            "Example name": "Sample",
            "Example point 1": 1,
            "Example point 2": 2,
            "Example point 3": 3,
            "Example point 4": 4,
            "Example point 5": 5,
        },
        {
            "Example name": "Sample 2",
            "Example point 1": 6,
            "Example point 2": 7,
            "Example point 3": 8,
            "Example point 4": 9,
            "Example point 5": 10,
        }
    );
</script>

<Label for="exampleFile">Cách hệ thống quét bảng</Label>
<Table>
    <thead>
        <tr>
            {#each Object.keys(example_data[0] || {}) as table_header}
                <th>{table_header}</th>
            {:else}
                <p>Không có thông tin xuất hiện</p>
            {/each}
        </tr>
    </thead>
    <tbody>
        {#each Object.values(example_data) as row}
            <tr>
                {#each Object.values(row) as cell}
                    <td>{cell}</td>
                {:else}
                    <p>Không có thông tin xuất hiện</p>
                {/each}
            </tr>
        {/each}
    </tbody>
</Table>
<svg width="100%" height="100">
    <defs>
        <marker
            id="arrow"
            markerWidth="10"
            markerHeight="10"
            refX="0"
            refY="3"
            orient="auto"
            markerUnits="strokeWidth"
        >
            <path d="M0,0 L0,6 L9,3 z" fill="black" />
        </marker>
    </defs>

    <polyline
        class="dash-tap"
        points="0,0 550,0 0,75 550,75"
        fill="none"
        stroke="black"
        marker-end="url(#arrow)"
    />
</svg>
<p class="info">
    Hệ thống quét từng ô theo hàng để kiểm tra hay nhận giá trị để tính toán,
    sau đó khi tất cả các ô trong hàng đã được quét xong, hệ thống sẽ đưa xuống
    hàng bên dưới và tiếp tục quét theo ô liên tục tùy theo số hàng trong bảng.
</p>
