import React, { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";
import { Select, SelectTrigger, SelectContent, SelectItem, SelectValue } from "@/components/ui/select";
import * as XLSX from "xlsx";

export default function OrderTracker() {
  const [projectName, setProjectName] = useState("");
  const [projectDate, setProjectDate] = useState("");
  const [requestedBy, setRequestedBy] = useState("");
  const [placedBy, setPlacedBy] = useState("");
  const [paymentMethod, setPaymentMethod] = useState("");
  const [searchTerm, setSearchTerm] = useState("");
  const [statusFilter, setStatusFilter] = useState("");
  const [form, setForm] = useState({
    name: "",
    sku: "",
    quantity: "",
    status: "",
    supplier: "",
    price: "",
    netCost: "",
    notes: "",
    returnPolicy: "",
    image: null,
    invoice: null
  });
  const [items, setItems] = useState([]);

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleImageChange = (e) => {
    setForm({ ...form, image: e.target.files[0] });
  };

  const addItem = () => {
    setItems([...items, { ...form }]);
    setForm({
      name: "",
      sku: "",
      quantity: "",
      status: "",
      supplier: "",
      price: "",
      netCost: "",
      notes: "",
      returnPolicy: "",
      image: null,
      invoice: null
    });
  };

  const saveToExcel = () => {
    const wb = XLSX.utils.book_new();
    const wsData = [
      ["Name", "SKU", "Quantity", "Status", "Supplier", "Unit Price", "Net Price", "Notes", "Return Policy", "Image Filename"]
    ];

    items.forEach(item => {
      wsData.push([
        item.name,
        item.sku,
        item.quantity,
        item.status,
        item.supplier,
        item.price,
        item.netCost,
        item.notes,
        item.returnPolicy,
        item.image ? item.image.name : ""
      ]);
    });

    const ws = XLSX.utils.aoa_to_sheet(wsData);
    XLSX.utils.book_append_sheet(wb, ws, "Orders");
    XLSX.writeFile(wb, "order_data.xlsx");
  };

  const printPage = () => {
    const newWindow = window.open("", "", "width=900,height=700");
    newWindow.document.write(`<!DOCTYPE html><html><head><title>Print</title><style>
      body { font-family: Arial, sans-serif; padding: 20px; background-color: #111827; color: #d1d5db; }
      table { border-collapse: collapse; width: 100%; margin-top: 20px; }
      th { background-color: #f97316; color: white; padding: 8px; border: 1px solid #ccc; text-align: left; }
      td { background: #1f2937; color: #d1d5db; border: 1px solid #ccc; padding: 8px; }
      h1 { color: #f97316; display: flex; justify-content: space-between; align-items: center; }
      .header-section { display: flex; justify-content: space-between; align-items: center; }
      img.print-image { width: 30px; height: 30px; object-fit: cover; }
      .watermark { font-size: 10px; text-align: center; margin-top: 40px; color: #aaa; }
    </style></head><body>`);
    newWindow.document.write(`<div class="header-section">
      <h1><span style='color: #f97316; font-weight: bold;'>Sensationsexhibits</span> - Order Tracker</h1>
    </div>`);
    newWindow.document.write(`<p><strong>Project Name:</strong> ${projectName}</p>`);
    newWindow.document.write(`<p><strong>Project Date:</strong> ${projectDate}</p>`);
    newWindow.document.write(`<p><strong>Requested By:</strong> ${requestedBy}</p>`);
    newWindow.document.write(`<p><strong>Placed By:</strong> ${placedBy}</p>`);
    newWindow.document.write(`<p><strong>Payment Method:</strong> ${paymentMethod}</p>`);
    newWindow.document.write(`<p><strong>Timestamp:</strong> ${new Date().toLocaleString()}</p>`);

    const thead = `<thead><tr><th>Name</th><th>SKU</th><th>Qty</th><th>Status</th><th>Supplier</th><th>Price</th><th>Net Cost</th><th>Notes</th><th>Return</th><th>Image</th></tr></thead>`;
    let tbody = "<tbody>";
    items.forEach(item => {
      const imageURL = item.image ? URL.createObjectURL(item.image) : "";
      tbody += `<tr>
        <td>${item.name}</td>
        <td>${item.sku}</td>
        <td>${item.quantity}</td>
        <td>${item.status}</td>
        <td>${item.supplier}</td>
        <td>$${item.price}</td>
        <td>${item.netCost}</td>
        <td>${item.notes}</td>
        <td>${item.returnPolicy}</td>
        <td>${imageURL ? `<img src="${imageURL}" alt="Item" class="print-image" />` : ""}</td>
      </tr>`;
    });
    tbody += "</tbody>";

    newWindow.document.write(`<table>${thead}${tbody}</table>`);
    newWindow.document.write("<div class='watermark'>Made by Gautam Aneja in collaboration with Sensationsexhibits</div>");
    newWindow.document.write("</body></html>");
    newWindow.document.close();
    setTimeout(() => newWindow.print(), 500);
  };

  const filteredItems = items.filter(item =>
    (item.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
     item.sku.toLowerCase().includes(searchTerm.toLowerCase())) &&
    (statusFilter === "" || item.status === statusFilter)
  );

  return (
    <div className="p-4 space-y-6 bg-[#0a0f1a] min-h-screen text-white">
      <div className="flex justify-between items-center mb-4">
        <h1 className="text-2xl font-bold text-orange-500">Sensationsexhibits</h1>
        <h2 className="text-xl font-bold text-white">Order Placement</h2>
      </div>

      <div className="space-y-2 bg-[#0a0f1a]">
        <div className="grid md:grid-cols-5 gap-4">
          <Input className="bg-[#0a0f1a] text-white" placeholder="Project Name" value={projectName} onChange={(e) => setProjectName(e.target.value)} />
          <Input className="bg-[#0a0f1a] text-white" type="datetime-local" value={projectDate} onChange={(e) => setProjectDate(e.target.value)} />
          <Input className="bg-[#0a0f1a] text-white" placeholder="Requested By" value={requestedBy} onChange={(e) => setRequestedBy(e.target.value)} />
          <Input className="bg-[#0a0f1a] text-white" placeholder="Placed By" value={placedBy} onChange={(e) => setPlacedBy(e.target.value)} />
          <Input className="bg-[#0a0f1a] text-white" placeholder="Payment Method" value={paymentMethod} onChange={(e) => setPaymentMethod(e.target.value)} />
        </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-1 lg:grid-cols-1 gap-4 pt-4">
        <Card className="bg-[#0a0f1a] border border-orange-500">
          <CardContent className="grid grid-cols-1 md:grid-cols-2 gap-4 p-4">
            <Input className="bg-[#0a0f1a] text-white" name="name" placeholder="Item Name" value={form.name} onChange={handleChange} />
            <Input className="bg-[#0a0f1a] text-white" name="sku" placeholder="SKU" value={form.sku} onChange={handleChange} />
            <Input className="bg-[#0a0f1a] text-white" name="quantity" placeholder="Quantity" type="number" value={form.quantity} onChange={handleChange} />
            <Input className="bg-[#0a0f1a] text-white" name="price" placeholder="Unit Price" type="number" value={form.price} onChange={handleChange} />
            <Input className="bg-[#0a0f1a] text-white" name="netCost" placeholder="Net Price" type="number" value={form.netCost} onChange={handleChange} />
            <Input className="bg-[#0a0f1a] text-white" name="supplier" placeholder="Supplier" value={form.supplier} onChange={handleChange} />
            <Select name="status" value={form.status} onValueChange={(val) => setForm({ ...form, status: val })}>
              <SelectTrigger className="bg-[#0a0f1a] text-white"><SelectValue placeholder="Select Status" /></SelectTrigger>
              <SelectContent>
                <SelectItem value="Pending">🔴 Pending</SelectItem>
                <SelectItem value="Ordered">🟠 Ordered</SelectItem>
                <SelectItem value="Delivered">🟢 Delivered</SelectItem>
              </SelectContent>
            </Select>
            <Select name="returnPolicy" value={form.returnPolicy} onValueChange={(val) => setForm({ ...form, returnPolicy: val })}>
              <SelectTrigger className="bg-[#0a0f1a] text-white"><SelectValue placeholder="Return Policy" /></SelectTrigger>
              <SelectContent>
                <SelectItem value="7 Days">7 Days</SelectItem>
                <SelectItem value="30 Days">30 Days</SelectItem>
                <SelectItem value="Non-Refundable">Non-Refundable</SelectItem>
              </SelectContent>
            </Select>
            <Input className="bg-[#0a0f1a] text-white" name="notes" placeholder="Notes" value={form.notes} onChange={handleChange} />
            <Input type="file" onChange={handleImageChange} className="bg-[#0a0f1a] text-orange-500" />
            {form.image && <img src={URL.createObjectURL(form.image)} alt="preview" className="h-16 w-16 object-cover border border-gray-300" />}
            <Button onClick={addItem} className="bg-orange-500 text-white w-full mt-2">Add Item</Button>
          </CardContent>
        </Card>
      </div>

      <Table className="mt-6">
        <TableHeader>
          <TableRow className="bg-[#0a0f1a] text-orange-500">
            <TableHead>Name</TableHead>
            <TableHead>SKU</TableHead>
            <TableHead>Quantity</TableHead>
            <TableHead>Status</TableHead>
            <TableHead>Supplier</TableHead>
            <TableHead>Unit Price</TableHead>
            <TableHead>Net Price</TableHead>
            <TableHead>Notes</TableHead>
            <TableHead>Return</TableHead>
            <TableHead>Image</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {filteredItems.map((item, index) => (
            <TableRow key={index} className="bg-[#1f2937] text-white">
              <TableCell>{item.name}</TableCell>
              <TableCell>{item.sku}</TableCell>
              <TableCell>{item.quantity}</TableCell>
              <TableCell>{item.status}</TableCell>
              <TableCell>{item.supplier}</TableCell>
              <TableCell>{item.price}</TableCell>
              <TableCell>{item.netCost}</TableCell>
              <TableCell>{item.notes}</TableCell>
              <TableCell>{item.returnPolicy}</TableCell>
              <TableCell>{item.image ? item.image.name : ""}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>

      <div className="flex justify-end space-x-4 pt-6">
        <Button onClick={printPage} className="bg-orange-500 text-white rounded px-6 py-2">Print</Button>
        <Button onClick={saveToExcel} className="bg-orange-500 text-white rounded px-6 py-2">Save</Button>
      </div>

      <p className="text-xs text-gray-500 text-center pt-6">Made by Gautam Aneja in collaboration with Sensationsexhibits</p>
    </div>
  );
}
