<script>
    // @ts-nocheck
    import {
        Form,
        FormGroup,
        FormText,
        Input,
        Label,
        Button,
        Card,
        CardBody,
        CardHeader,
        CardSubtitle,
        CardText,
        CardFooter,
        CardTitle,
        Table,
    } from "sveltestrap";

    import readXlsxFile from "read-excel-file";
    import * as Excel from "exceljs";
    import fileSaver from "file-saver";

    import {
        Chart,
        Title,
        Tooltip,
        Legend,
        ArcElement,
        CategoryScale,
    } from "chart.js";
    import { Pie } from "svelte-chartjs";

    Chart.register(Title, Tooltip, Legend, ArcElement, CategoryScale);

    let xlsx_file = $state(), warn_string = $state(), alert_string = $state();
    var eval_result_table = $state([]),
        calc_result_table = $state([]),
        calc_result_table_redacted = [];
    var present_counter = 0,
        null_counter = 0,
        pass_counter = $state(0),
        fail_counter = $state(0),
        disabled = $state(true);

    const UpEval = () => {
        let file_name_ext = xlsx_file.item(0).name;
        var title_string = [],
            process_cache = [],
            process_string = [],
            table_string = [],
            eval_result = [],
            eval_result_completed = [];

        if (file_name_ext === "Mau diem01-IT17301.xlsx") {
            readXlsxFile(xlsx_file.item(0)).then(function (row) {
                row[7].forEach((cell) => {
                    title_string.push(cell);
                });

                for (let i = 8; i <= 47; i++) {
                    row[i].forEach((cell) => {
                        process_cache.push(cell);
                    });
                    process_string = process_cache;
                }
                while (process_string.length) {
                    table_string.push(process_string.splice(0, 21));
                }
                for (let col = 0; col < 40; col++) {
                    for (let row = 3; row < 21; row++) {
                        const check_cell = table_string[col][row] !== null;
                        if (check_cell === true) {
                            eval_result_completed.push({
                                "Mã sinh viên": table_string[col][1],
                                "Tên sinh viên": table_string[col][2],
                                "Phạm vi trong bảng": title_string[row],
                                "Tình trạng giá trị": "Có thông tin",
                            });
                        } else {
                            eval_result.push({
                                "Mã sinh viên": table_string[col][1],
                                "Tên sinh viên": table_string[col][2],
                                "Phạm vi trong bảng": title_string[row],
                                "Tình trạng giá trị": "Thiếu thông tin",
                            });
                        }
                    }

                    if (
                        table_string[col][3] !== null &&
                        table_string[col][4] !== null &&
                        table_string[col][5] !== null &&
                        table_string[col][6] !== null &&
                        table_string[col][7] !== null &&
                        table_string[col][8] !== null &&
                        table_string[col][9] !== null &&
                        table_string[col][10] !== null &&
                        table_string[col][11] !== null &&
                        table_string[col][12] !== null &&
                        table_string[col][13] !== null &&
                        table_string[col][14] !== null &&
                        table_string[col][15] !== null &&
                        table_string[col][16] !== null &&
                        table_string[col][17] !== null &&
                        table_string[col][18] !== null &&
                        table_string[col][19] !== null &&
                        table_string[col][20] !== null
                    ) {
                        eval_result.push({
                            "Mã sinh viên": table_string[col][1],
                            "Tên sinh viên": table_string[col][2],
                            "Phạm vi trong bảng": "Toàn hàng",
                            "Tình trạng giá trị": "Có thông tin",
                        });

                        present_counter++;
                    } else {
                        null_counter++;
                    }
                    eval_result_table = eval_result;
                }
                alert_string =
                    `<p>Có ${present_counter}/40 sinh viên đã đủ thông tin</p>` +
                    `<p>Có ${null_counter}/40 sinh viên thiếu thông tin</p>`;
                
                if (present_counter === 40) {
                    disabled = false;
                } else {
                    warn_string =
                        "<p>Đã kiểm tra xong với vài cột điểm còn thiếu</p>";
                }
            });
        } else if (file_name_ext === "MauDiem02-MA16301.xlsx") {
            readXlsxFile(xlsx_file.item(0)).then(function (row) {
                row[7].forEach((cell) => {
                    title_string.push(cell);
                });

                for (let i = 8; i <= 28; i++) {
                    row[i].forEach((cell) => {
                        process_cache.push(cell);
                    });
                    process_string = process_cache;
                }
                while (process_string.length) {
                    table_string.push(process_string.splice(0, 8));
                }
                for (let col = 0; col < 21; col++) {
                    for (let row = 3; row < 8; row++) {
                        const check_cell = table_string[col][row] !== null;
                        if (check_cell === true) {
                            eval_result_completed.push({
                                "Mã sinh viên": table_string[col][1],
                                "Tên sinh viên": table_string[col][2],
                                "Phạm vi trong bảng": title_string[row],
                                "Tình trạng giá trị": "Có thông tin",
                            });
                        } else {
                            eval_result.push({
                                "Mã sinh viên": table_string[col][1],
                                "Tên sinh viên": table_string[col][2],
                                "Phạm vi trong bảng": title_string[row],
                                "Tình trạng giá trị": "Thiếu thông tin",
                            });
                        }
                    }

                    if (
                        table_string[col][3] !== null &&
                        table_string[col][4] !== null &&
                        table_string[col][5] !== null &&
                        table_string[col][6] !== null &&
                        table_string[col][7] !== null
                    ) {
                        eval_result.push({
                            "Mã sinh viên": table_string[col][1],
                            "Tên sinh viên": table_string[col][2],
                            "Phạm vi trong bảng": "Toàn hàng",
                            "Tình trạng giá trị": "Có thông tin",
                        });

                        present_counter++;
                    } else {
                        null_counter++;
                    }
                    eval_result_table = eval_result;
                }
                alert_string =
                    `<p>Có ${present_counter}/21 sinh viên đã đủ thông tin</p>` +
                    `<p>Có ${null_counter}/21 sinh viên thiếu thông tin</p>`;
                
                if (present_counter === 21) {
                    disabled = false;
                } else {
                    warn_string =
                        "<p>Đã kiểm tra xong với vài cột điểm còn thiếu</p>";
                }
            });
        } else {
            readXlsxFile(xlsx_file.item(0)).then(function (row) {
                console.log(row);
                warn_string =
                    "<p>Chức năng trên không hỗ trợ trong bảng trên</p>";
            });
        }
    };

    function UpCalcPrint() {
        let file_name_ext = xlsx_file.item(0).name;
        var process_cache = [],
            process_string = [],
            table_string = [],
            res_array = [],
            res_array_60 = [],
            final_array = [],
            final_array_60 = [],
            qualify_string = [],
            calc_result = [];

        const workbook = new Excel.Workbook();
        const worksheet = workbook.addWorksheet("Result sheet");

        if (file_name_ext === "Mau diem01-IT17301.xlsx") {
            readXlsxFile(xlsx_file.item(0)).then(function (row) {
                for (let i = 8; i <= 47; i++) {
                    row[i].forEach((cell) => {
                        process_cache.push(cell);
                    });
                    process_string = process_cache;
                }
                while (process_string.length) {
                    table_string.push(process_string.splice(0, 21));
                }
                for (let col = 0; col < 40; col++) {
                    res_array.push(
                        (
                            (table_string[col][3] * 10) / 100 +
                            (table_string[col][4] * 2) / 100 +
                            (table_string[col][5] * 2) / 100 +
                            (table_string[col][6] * 2) / 100 +
                            (table_string[col][7] * 2) / 100 +
                            (table_string[col][8] * 2) / 100 +
                            (table_string[col][9] * 2) / 100 +
                            (table_string[col][10] * 2) / 100 +
                            (table_string[col][11] * 2) / 100 +
                            (table_string[col][12] * 10) / 100 +
                            (table_string[col][13] * 4) / 100 +
                            (table_string[col][14] * 4) / 100 +
                            (table_string[col][15] * 4) / 100 +
                            (table_string[col][16] * 4) / 100 +
                            (table_string[col][17] * 4) / 100 +
                            (table_string[col][18] * 4) / 100 +
                            (table_string[col][19] * 4) / 100 +
                            (table_string[col][20] * 4) / 100
                        ).toFixed(2)
                    );
                    res_array_60.push(((res_array[col] * 100) / 60).toFixed(2));
                }

                final_array = res_array;
                final_array_60 = res_array_60;

                for (let col_calc = 0; col_calc < 40; col_calc++) {
                    calc_result.push({
                        "Sinh viên": table_string[col_calc][2],
                        "Điểm tổng": final_array[col_calc],
                        "Tổng điểm": final_array_60[col_calc],
                    });

                    if (final_array_60[col_calc] >= 5) {
                        qualify_string.push("Đủ điều kiện thi");
                        pass_counter++;
                    } else {
                        qualify_string.push("Thiếu điều kiện thi");
                        fail_counter++;
                    }

                    calc_result_table_redacted.push({
                        student: table_string[col_calc][2],
                        total_score: final_array[col_calc],
                        final_score: final_array_60[col_calc],
                        is_qualified: qualify_string[col_calc],
                    });
                }

                calc_result_table = calc_result;

                alert_string =
                    `<p>Có ${pass_counter}/40 sinh viên đã đủ điều kiện thi</p>` +
                    `<p>Có ${fail_counter}/40 sinh viên thiếu điều kiện thi</p>`;

                worksheet.columns = [
                    {
                        header: "Họ và tên",
                        key: "name",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Assignment 1",
                        key: "asm_s1",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 1",
                        key: "lab_1",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 2",
                        key: "lab_2",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 3",
                        key: "lab_3",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 4",
                        key: "lab_4",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 5",
                        key: "lab_5",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 6",
                        key: "lab_6",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 7",
                        key: "lab_7",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Lab 8",
                        key: "lab_8",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Assignment 2",
                        key: "asm_s2",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 1",
                        key: "quiz_1",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 2",
                        key: "quiz_2",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 3",
                        key: "quiz_3",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 4",
                        key: "quiz_4",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 5",
                        key: "quiz_5",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 6",
                        key: "quiz_6",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 7",
                        key: "quiz_7",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Quiz 8",
                        key: "quiz_8",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Điểm theo 100%",
                        key: "total_score",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Điểm trung bình theo hệ số %",
                        key: "final_score",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Dự thi",
                        key: "is_qualified",
                        width: 30,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                ];

                for (let print_col = 0; print_col < 40; print_col++) {
                    worksheet.addRow({
                        name: table_string[print_col][2],
                        asm_s1: table_string[print_col][3],
                        lab_1: table_string[print_col][4],
                        lab_2: table_string[print_col][5],
                        lab_3: table_string[print_col][6],
                        lab_4: table_string[print_col][7],
                        lab_5: table_string[print_col][8],
                        lab_6: table_string[print_col][9],
                        lab_7: table_string[print_col][10],
                        lab_8: table_string[print_col][11],
                        asm_s2: table_string[print_col][12],
                        quiz_1: table_string[print_col][13],
                        quiz_2: table_string[print_col][14],
                        quiz_3: table_string[print_col][15],
                        quiz_4: table_string[print_col][16],
                        quiz_5: table_string[print_col][17],
                        quiz_6: table_string[print_col][18],
                        quiz_7: table_string[print_col][19],
                        quiz_8: table_string[print_col][20],
                        total_score:
                            calc_result_table_redacted[print_col].total_score,
                        final_score:
                            calc_result_table_redacted[print_col].final_score,
                        is_qualified:
                            calc_result_table_redacted[print_col].is_qualified,
                    });
                }

                workbook.xlsx
                    .writeBuffer()
                    .then((buffer) =>
                        fileSaver.saveAs(
                            new Blob([buffer]),
                            `IT17301_${Date.now()}_calc_print_result.xlsx`
                        )
                    )
                    .catch((err) =>
                        console.log("Error printing excel export", err)
                    );
            });
        } else if (file_name_ext === "MauDiem02-MA16301.xlsx") {
            readXlsxFile(xlsx_file.item(0)).then(function (row) {
                for (let i = 8; i <= 28; i++) {
                    row[i].forEach((cell) => {
                        process_cache.push(cell);
                    });
                    process_string = process_cache;
                }
                while (process_string.length) {
                    table_string.push(process_string.splice(0, 8));
                }
                for (let col = 0; col < 21; col++) {
                    res_array.push(
                        (
                            (table_string[col][3] * 10) / 100 +
                            (table_string[col][4] * 10) / 100 +
                            (table_string[col][5] * 14) / 100 +
                            (table_string[col][6] * 14) / 100 +
                            (table_string[col][7] * 12) / 100
                        ).toFixed(2)
                    );
                    res_array_60.push(((res_array[col] * 100) / 60).toFixed(2));
                }

                final_array = res_array;
                final_array_60 = res_array_60;

                for (let col_calc = 0; col_calc < 21; col_calc++) {
                    calc_result.push({
                        "Sinh viên": table_string[col_calc][2],
                        "Điểm tổng": final_array[col_calc],
                        "Tổng điểm": final_array_60[col_calc],
                    });

                    if (final_array_60[col_calc] >= 5) {
                        qualify_string.push("Đủ điều kiện thi");
                        pass_counter++;
                    } else {
                        qualify_string.push("Thiếu điều kiện thi");
                        fail_counter++;
                    }

                    calc_result_table_redacted.push({
                        student: table_string[col_calc][2],
                        total_score: final_array[col_calc],
                        final_score: final_array_60[col_calc],
                        is_qualified: qualify_string[col_calc],
                    });
                }

                calc_result_table = calc_result;

                alert_string =
                    `<p>Có ${pass_counter}/21 sinh viên đã đủ điều kiện thi</p>` +
                    `<p>Có ${fail_counter}/21 sinh viên thiếu điều kiện thi</p>`;

                worksheet.columns = [
                    {
                        header: "Họ và tên",
                        key: "name",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Assignment 1",
                        key: "asm_s1",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Assignment 2",
                        key: "asm_s2",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Progress test 1",
                        key: "pgr_s1",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Progress test 2",
                        key: "pgr_s2",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Online test",
                        key: "oln",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Điểm theo 100%",
                        key: "total_score",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Điểm trung bình theo hệ số %",
                        key: "final_score",
                        width: 20,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                    {
                        header: "Dự thi",
                        key: "is_qualified",
                        width: 30,
                        style: {
                            font: {
                                name: "Times New Roman",
                                size: 11,
                                bold: true,
                            },
                            alignment: {
                                vertical: "middle",
                                horizontal: "left",
                            },
                        },
                    },
                ];

                for (let print_col = 0; print_col < 21; print_col++) {
                    worksheet.addRow({
                        name: table_string[print_col][2],
                        asm_s1: table_string[print_col][3],
                        asm_s2: table_string[print_col][4],
                        pgr_s1: table_string[print_col][5],
                        pgr_s2: table_string[print_col][6],
                        oln: table_string[print_col][7],
                        total_score:
                            calc_result_table_redacted[print_col].total_score,
                        final_score:
                            calc_result_table_redacted[print_col].final_score,
                        is_qualified:
                            calc_result_table_redacted[print_col].is_qualified,
                    });
                }

                workbook.xlsx
                    .writeBuffer()
                    .then((buffer) =>
                        fileSaver.saveAs(
                            new Blob([buffer]),
                            `MA16301_${Date.now()}_calc_print_result.xlsx`
                        )
                    )
                    .catch((err) =>
                        console.log("Error printing excel export", err)
                    );
            });
        } else {
            readXlsxFile(xlsx_file.item(0)).then(function (row) {
                console.log(row);
                warn_string =
                    "<p>Chức năng trên không hỗ trợ trong bảng trên</p>";
            });
        }
    }
</script>

<Form>
    <FormGroup method="get" class="reader">
        <Label for="exampleFile">Nhập file Excel tại đây</Label>
        <Input
            type="file"
            id="xlsx"
            bind:files={xlsx_file}
            accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, 
            application/vnd.ms-excel"
        />
        <Button type="button" on:click={UpEval}>Tải lên và kiểm tra</Button>
        <Button type="button" on:click={UpCalcPrint} {disabled}
            >Tải lên, tính toán và xuất bản</Button
        >
        <FormText color="muted">
            Tùy theo bảng mà bạn nhập, hệ thống sẽ kiểm tra và tính toán một
            cách linh hoạt theo bảng trên.
        </FormText>
    </FormGroup>
</Form>
<Card>
    <CardHeader>
        <CardTitle>Thẻ kiểm tra tiến độ</CardTitle>
    </CardHeader>
    <CardBody>
        <CardSubtitle>Các bạn kiểm tra tiến độ tại đây.</CardSubtitle>
        <CardText>Bảng kết quả kiểm tra</CardText>
        <Table>
            <thead>
                <tr>
                    {#each Object.keys(eval_result_table[0] || {}) as table_header}
                        <th>{table_header}</th>
                    {:else}
                        <p>Không có thông tin xuất hiện</p>
                    {/each}
                </tr>
            </thead>
            <tbody>
                {#each Object.values(eval_result_table) as row}
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
        <CardText>Phần cảnh báo</CardText>
        <div contenteditable="true" bind:innerHTML={warn_string}>
            <p>Không có cảnh báo nào</p>
        </div>
        <CardText>Phần tổng quát</CardText>
        <CardText class="summary">
            <div class="summary_content">
                <ul contenteditable="true" bind:innerHTML={alert_string}></ul>
            </div>
        </CardText>
        <CardText>Bảng kết quả tính toán</CardText>
        <Table>
            <thead>
                <tr>
                    {#each Object.keys(calc_result_table[0] || {}) as table_header}
                        <th>{table_header}</th>
                    {:else}
                        <p>Không có thông tin xuất hiện</p>
                    {/each}
                </tr>
            </thead>
            <tbody>
                {#each Object.values(calc_result_table) as row}
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
        <CardText>Thống kê tính toán</CardText>
        <div class="chart">
            <Pie
                data={{
                    labels: ["Đủ điều kiện", "Không đủ điều kiện"],
                    datasets: [
                        {
                            label: "Số lượng học sinh đủ/không đủ điều kiện thi",
                            data: [pass_counter, fail_counter],
                            backgroundColor: [
                                "rgb(54, 162, 235)",
                                "rgb(255, 99, 132)",
                            ],
                            hoverOffset: 4,
                        },
                    ],
                }}
                options={{ responsive: false }}
            />
        </div>
    </CardBody>
    <CardFooter>Đoạn cuối của thẻ</CardFooter>
</Card>
