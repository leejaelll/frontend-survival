---
description: JSON데이터 엑셀 파일로 내보내기
---

# JSON to XLSX

<details>

<summary>View Code</summary>



```typescript
import { IconDownload, IconDownloadData } from "../ui/icons";
import { Button, DatePicker, message, Modal } from "antd";
import dayjs, { Dayjs } from "dayjs";
import { useEffect, useState } from "react";
import * as XLSX from "xlsx";

type DayType = [Dayjs | null, Dayjs | null] | null;

export default function PromptDatePicker() {
  const { RangePicker } = DatePicker;
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [date, setDate] = useState<{ startDate: DayType; endDate: DayType }>({
    startDate: null,
    endDate: null,
  });
  const [error, setError] = useState<string>();

  useEffect(() => {
    if (error) {
      message.error(error);
    }
  }, [error]);

  const showModal = () => {
    setIsModalOpen(true);
  };

  const renameKey = (obj: any, oldKey: string, newKey: string) => {
    obj[newKey] = obj[oldKey];
    delete obj[oldKey];
  };

  const formatDateToYYYYMMDD = (dateString: string) => {
    const date = new Date(dateString);
    return dayjs(date).format("YYYY-MM-DD HH:mm:ss");
  };

  const createWorksheetWithHeaders = (headers: string[]) => {
    const worksheet = XLSX.utils.aoa_to_sheet([headers]);
    return worksheet;
  };

  const processAndRearrangeData = (data: any[], columnsOrder: string[]) => {
    return data.map((item) => {
      const reorderedItem: any = {};
      columnsOrder.forEach((key) => {
        reorderedItem[key] = item[key];
      });
      return reorderedItem;
    });
  };

  const downloadExcel = (data: any) => {
    const columnsOrder = ["date", "original", "english", "language", "domain", "user id"];

    const workbook = XLSX.utils.book_new();

    if (!data.length) {
      const worksheet = createWorksheetWithHeaders(columnsOrder);
      XLSX.utils.book_append_sheet(workbook, worksheet, "Sheet1");
    } else {
      const arrangedData = data.map((item: any) => {
        renameKey(item, "createdAt", "date");
        renameKey(item, "userId", "user id");

        if (item.date) {
          item.date = formatDateToYYYYMMDD(item.date);
        }

        return item;
      });

      const rearrangedData = processAndRearrangeData(arrangedData, columnsOrder);

      const worksheet = XLSX.utils.json_to_sheet(rearrangedData);

      worksheet["!cols"] = Array(columnsOrder.length).fill({ wch: 20 });

      // worksheet["!cols"] = [{ wch: 20 }];
      XLSX.utils.book_append_sheet(workbook, worksheet, "Sheet1");
    }

    XLSX.writeFile(workbook, "data.xlsx");
  };

  const handleOk = async () => {
    setIsModalOpen(false);

    try {
      const response = await fetch("/api/prompt", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(date),
      });

      const data = await response.json();
      const { item } = data;

      downloadExcel(item);
    } catch (e: any) {
      console.log(e.error);
      setError(e?.error?.name ?? "Something went wrong");
    }
  };

  const handleCancel = () => {
    setIsModalOpen(false);
  };

  const handleDateChange = (value: any) => {
    if (value && value[0] && value[1]) {
      setDate({ startDate: value[0].format("YYYY-MM-DD"), endDate: value[1].format("YYYY-MM-DD") });
    } else {
      setDate({ startDate: null, endDate: null });
      console.log("No date range selected");
    }
  };

  return (
    <>
      <Button onClick={showModal} className="ml-auto border-none rounded-md">
        <IconDownloadData />
      </Button>
      <Modal
        title="Get User Question History"
        open={isModalOpen}
        onOk={handleOk}
        onCancel={handleCancel}
        styles={{
          content: { borderRadius: "10px", padding: "35px 50px" },
          body: {},
        }}
        footer={[
          <Button key="back" onClick={handleCancel} className="rounded-[8px]">
            Cancel
          </Button>,
          <Button key="submit" type="primary" onClick={handleOk} disabled={!date.startDate || !date.endDate} className="rounded-[8px]">
            OK
          </Button>,
        ]}
      >
        <div className="px-[40px] py-[25px]">
          <p className="mt-0 mb-[12px] opacity-[45%]">Select the date to get data</p>
          <RangePicker className="w-full rounded-[4px]" onChange={handleDateChange} />
        </div>
      </Modal>
    </>
  );
}

```

</details>

## `XLSX.utils.json_to_sheet()`

: JSON 데이터를 시트 데이터로 변환

```
const worksheet = XLSX.utils.json_to_sheet(data);
```



## `XLSX.utils.book_new()`

: 새로운 워크북 생성

```
const workbook = XLSX.utils.book_new();
```



## `XLSX.utils.book_append_sheet()`

: 워크북에 시트 추가

```
XLSX.utils.book_append_sheet(workbook, worksheet, "Sheet1");
```



## `XLSX.writeFile()`

: 엑셀 파일 생성 및 다운로드

```
XLSX.writeFile(workbook, "data.xlsx");
```
