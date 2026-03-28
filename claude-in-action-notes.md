# 🤖 Claude Code – Tổng Quan Khóa Học

> Ghi chú đầy đủ về cách làm việc với Claude Code – từ khái niệm cơ bản đến kỹ thuật nâng cao.

---

## 1. Coding Assistant là gì?

Coding Assistant là công cụ sử dụng language model để viết code và hoàn thành các tác vụ phát triển phần mềm.

```mermaid
flowchart TD
    A([👤 Developer]) -->|Gửi task| B[🤖 Coding Assistant]
    B --> C[🧠 Language Model thu thập context]
    C --> D[📂 Đọc file, hiểu codebase]
    D --> E[📋 Lên kế hoạch giải quyết]
    E --> F[⚡ Thực thi hành động]
    F --> G{Hoàn thành?}
    G -->|Chưa – cần thêm info| C
    G -->|Rồi| H([✅ Kết quả: file cập nhật, test chạy])

    classDef user fill:#4f46e5,color:#fff,stroke:#3730a3,stroke-width:2px
    classDef ai fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef process fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef success fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px
    classDef decision fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:2px

    class A user
    class B,C ai
    class D,E,F process
    class G decision
    class H success
```

### Hệ thống Tool Use

Language model **chỉ xử lý text** – không thể trực tiếp đọc file hay chạy lệnh. Tool Use là cầu nối:

```mermaid
sequenceDiagram
    autonumber
    actor Dev as 👤 Developer
    participant Asst as 🤖 Assistant
    participant LM as 🧠 Language Model
    participant Sys as 💻 System

    Dev->>+Asst: Gửi yêu cầu
    Asst->>+LM: Yêu cầu + hướng dẫn tool
    Note over LM: Phân tích context<br/>Chọn tool phù hợp
    LM-->>-Asst: Formatted action (vd: "read file: main.py")
    Asst->>+Sys: Thực thi hành động thật
    Sys-->>-Asst: Kết quả (nội dung file, output lệnh...)

    loop Lặp nếu cần thêm tool calls
        Asst->>+LM: Gửi kết quả + yêu cầu tiếp
        Note over LM: Quyết định bước tiếp theo
        LM-->>-Asst: Action tiếp hoặc phản hồi cuối
        opt Cần thêm tool
            Asst->>+Sys: Thực thi hành động tiếp
            Sys-->>-Asst: Kết quả mới
        end
    end

    Asst-->>-Dev: ✅ Phản hồi cuối cùng
```

**Ưu điểm của Claude:** Vượt trội về tool use, bảo mật cao hơn (tìm kiếm code trực tiếp, không gửi codebase ra server ngoài).

---

## 2. Claude Code trong thực tế

```mermaid
mindmap
  root((Claude Code))
    Demo hiệu năng
      Phân tích thư viện Chalk
        429M downloads/tuần
      Tăng throughput 3.9x
        Benchmark results
    Demo phân tích dữ liệu
      CSV churn analysis
      Jupyter Notebook
      Lặp phân tích theo kết quả
        Tự cải thiện output
    Mở rộng Tool
      Playwright MCP Server
        Browser automation
        UI iteration
      Tùy chỉnh tools
    GitHub Integration
      Chạy trong GitHub Actions
        CI/CD pipeline
      Trigger bởi PR/Issue
        @mention support
      Phát hiện PII exposure
        Security review
```

---

## 3. Quản lý Context

> **Nguyên tắc vàng:** Cung cấp vừa đủ thông tin liên quan – quá nhiều context không cần thiết sẽ làm giảm hiệu suất.

```mermaid
flowchart LR
    subgraph Claude_md["📄 File Claude.md"]
        direction TB
        A["🌐 Project Level<br/>Chia sẻ với team<br/>Commit vào source control"]
        B["👤 Local Level<br/>Cá nhân<br/>Không commit"]
        C["🖥️ Machine Level<br/>Toàn cục<br/>Mọi project"]
    end

    D["/init command<br/>📊 Phân tích toàn bộ codebase"] -->|Tạo tự động| Claude_md
    E["# symbol<br/>🧠 Memory Mode"] -->|Chỉnh sửa thông minh| Claude_md
    F["@ symbol<br/>📎 Mention file"] -->|Context có mục tiêu| G([🎯 Claude nhận context chính xác])
    Claude_md --> G

    classDef cmd fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef file fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef result fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px

    class D,E,F cmd
    class A,B,C file
    class G result

    style Claude_md fill:#f0f9ff,stroke:#0891b2,stroke-width:2px
```

---

## 4. Thực hiện thay đổi

```mermaid
flowchart TD
    A([🔍 Vấn đề cần thay đổi]) --> B{Loại thay đổi?}

    B -->|UI/Visual| C["📸 Chụp screenshot<br/>Ctrl+V để paste"]
    B -->|Multi-step rộng| D["🗺️ Plan Mode<br/>Shift+Tab ×2"]
    B -->|Logic phức tạp sâu| E["🧠 Thinking Mode<br/>'Ultra think'"]
    B -->|Cả hai| F[🔀 Kết hợp Plan + Thinking]

    C --> G[📝 Mô tả thay đổi mong muốn]
    D --> G
    E --> G
    F --> G

    G --> H[🤖 Claude thực hiện]
    H --> I{Review kết quả?}
    I -->|✅ Chấp nhận| J(["✅ Git commit<br/>với message mô tả"])
    I -->|❌ Từ chối| K[🔄 Điều chỉnh yêu cầu]
    K --> A

    classDef start fill:#4f46e5,color:#fff,stroke:#3730a3,stroke-width:2px
    classDef decision fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:2px
    classDef mode fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef action fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef success fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px
    classDef retry fill:#ef4444,color:#fff,stroke:#dc2626,stroke-width:2px

    class A start
    class B,I decision
    class C,D,E,F mode
    class G,H action
    class J success
    class K retry
```

---

## 5. Kiểm soát Context

```mermaid
flowchart LR
    subgraph controls["🎛️ Công cụ kiểm soát Context"]
        direction TB
        ESC["⎋ Escape<br/>Dừng Claude giữa chừng<br/>Để đổi hướng"]:::light
        ESC_MEM["⎋ + Memory (#)<br/>Dừng + ghi nhớ lỗi<br/>Ngăn lỗi lặp lại"]:::medium
        DBL_ESC["⎋⎋ Double Escape<br/>Tua lại hội thoại<br/>Bỏ qua back-and-forth"]:::strong
        COMPACT["/compact<br/>Tóm tắt lịch sử<br/>Giữ knowledge"]:::heavy
        CLEAR["/clear<br/>Xóa toàn bộ<br/>Task mới hoàn toàn"]:::nuclear
    end

    ESC -->|Nâng cấp| ESC_MEM
    ESC_MEM -->|Nâng cấp| DBL_ESC
    DBL_ESC -->|Nâng cấp| COMPACT
    COMPACT -->|Nâng cấp| CLEAR

    classDef light fill:#dbeafe,stroke:#3b82f6,stroke-width:2px,color:#1e40af
    classDef medium fill:#bfdbfe,stroke:#2563eb,stroke-width:2px,color:#1e3a8a
    classDef strong fill:#93c5fd,stroke:#1d4ed8,stroke-width:2px,color:#1e3a8a
    classDef heavy fill:#60a5fa,stroke:#1e40af,stroke-width:2px,color:#fff
    classDef nuclear fill:#3b82f6,stroke:#1e3a8a,stroke-width:3px,color:#fff

    style controls fill:#f8fafc,stroke:#64748b,stroke-width:2px
```

---

## 6. Custom Commands

```mermaid
flowchart TD
    A["📁 Tạo file Markdown<br/>trong .claude/commands/"]:::step --> B["📛 Tên file = tên lệnh<br/>VD: audit.md → /audit"]:::step
    B --> C["✏️ Viết instructions trong file<br/>Dùng $arguments cho tham số"]:::step
    C --> D["🔄 Restart Claude Code"]:::step
    D --> E["⌨️ Dùng /commandname args<br/>trong giao diện Claude"]:::step
    E --> F["🚀 Claude thực thi tự động"]:::success

    subgraph usecases["📦 Use Cases phổ biến"]
        direction LR
        U1["/audit<br/>🔍 Kiểm tra dependencies"]
        U2["/test<br/>🧪 Tạo unit tests"]
        U3["/fix<br/>🛡️ Vá lỗ hổng bảo mật"]
    end

    F --> usecases

    classDef step fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef success fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px

    style usecases fill:#fef3c7,stroke:#f59e0b,stroke-width:2px
    style U1 fill:#fef9c3,stroke:#eab308,stroke-width:1px
    style U2 fill:#fef9c3,stroke:#eab308,stroke-width:1px
    style U3 fill:#fef9c3,stroke:#eab308,stroke-width:1px
```

---

## 7. Mở rộng với MCP Servers

```mermaid
flowchart LR
    A["🤖 Claude Code"]:::main -->|claude mcp add| B["🔌 MCP Server"]:::server

    subgraph servers["🔌 MCP Servers có sẵn"]
        direction TB
        B1["🎭 Playwright<br/>Browser automation"]
        B2["🗄️ Database tools<br/>SQL queries"]
        B3["🌐 External APIs<br/>REST, GraphQL"]
    end

    B --> servers

    subgraph workflow["🔄 Workflow Playwright"]
        direction TB
        W1["1️⃣ Claude mở localhost:3000"]
        W2["2️⃣ Chụp screenshot UI"]
        W3["3️⃣ Phân tích chất lượng"]
        W4["4️⃣ Cập nhật generation prompt"]
        W5["5️⃣ Lặp lại → UI tốt hơn"]
        W1 --> W2 --> W3 --> W4 --> W5
        W5 -.->|Iterate| W1
    end

    B1 --> workflow

    C["⚙️ Auto-approve:<br/>Thêm MCP__servername<br/>vào settings.local.json"]:::config

    classDef main fill:#4f46e5,color:#fff,stroke:#3730a3,stroke-width:2px
    classDef server fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef config fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:2px

    style servers fill:#f0fdf4,stroke:#16a34a,stroke-width:2px
    style workflow fill:#eff6ff,stroke:#3b82f6,stroke-width:2px
    style B1 fill:#e0f2fe,stroke:#0284c7,stroke-width:1px
    style B2 fill:#e0f2fe,stroke:#0284c7,stroke-width:1px
    style B3 fill:#e0f2fe,stroke:#0284c7,stroke-width:1px
    style W1 fill:#dbeafe,stroke:#3b82f6,stroke-width:1px
    style W2 fill:#dbeafe,stroke:#3b82f6,stroke-width:1px
    style W3 fill:#dbeafe,stroke:#3b82f6,stroke-width:1px
    style W4 fill:#dbeafe,stroke:#3b82f6,stroke-width:1px
    style W5 fill:#dbeafe,stroke:#3b82f6,stroke-width:1px
```

---

## 8. GitHub Integration

```mermaid
flowchart TD
    A(["/install GitHub app"]):::start --> B["📦 Cài Claude Code App<br/>trên GitHub"]:::step
    B --> C["🔑 Thêm API Key"]:::step
    C --> D["⚡ Tự tạo 2 GitHub Actions"]:::step

    D --> E["💬 @mention support<br/>@Claude trong Issues/PRs"]:::feature
    D --> F["🔍 PR Review tự động<br/>Mỗi khi có PR mới"]:::feature

    E --> G["🤖 Claude nhận task<br/>và thực thi"]:::action
    F --> H["🤖 Claude review code<br/>và comment"]:::action

    subgraph customize["🔧 Tùy chỉnh"]
        C1["⚙️ Config trong<br/>.github/workflows"]
        C2["📝 Custom instructions<br/>cho Claude"]
        C3["🔌 Tích hợp<br/>MCP servers"]
    end

    G --> customize
    H --> customize

    classDef start fill:#4f46e5,color:#fff,stroke:#3730a3,stroke-width:2px
    classDef step fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef feature fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef action fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px

    style customize fill:#fef3c7,stroke:#f59e0b,stroke-width:2px
    style C1 fill:#fef9c3,stroke:#eab308,stroke-width:1px
    style C2 fill:#fef9c3,stroke:#eab308,stroke-width:1px
    style C3 fill:#fef9c3,stroke:#eab308,stroke-width:1px
```

**Ví dụ thực tế:** Claude tự động phát hiện lỗi **PII exposure** trong PR review khi phân tích luồng dữ liệu qua Lambda → S3 → đối tác bên ngoài.

---

## 9. Hooks – Kiểm soát Tool Calls

```mermaid
flowchart LR
    A["🤖 Claude định dùng Tool"]:::ai --> B{"🔒 Pre-tool Hook"}:::hook
    B -->|"Exit 0 ✅ Cho phép"| C["⚡ Tool thực thi"]:::tool
    B -->|"Exit 2 🚫 Chặn"| D["❌ Báo lỗi về Claude<br/>qua stderr"]:::error
    C --> E{"📋 Post-tool Hook"}:::hook
    E -->|Luôn chạy| F["🔄 Follow-up action<br/>VD: format, test, check"]:::action
    F --> G["📤 Phản hồi về Claude"]:::ai

    classDef ai fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef hook fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:2px
    classDef tool fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef error fill:#ef4444,color:#fff,stroke:#dc2626,stroke-width:2px
    classDef action fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px
```

### Cấu trúc Hook trong settings.local.json

```mermaid
flowchart TD
    subgraph config["⚙️ .claude/settings.local.json"]
        P["Pre-tool use hooks<br/>matcher: 'read|grep'<br/>command: 'node ./hooks/read_hook.js'"]:::pre
        Q["Post-tool use hooks<br/>matcher: 'write'<br/>command: 'node ./hooks/format_hook.js'"]:::post
    end

    subgraph hook_script["📜 Hook Script (Node.js)"]
        direction TB
        H1["📥 Đọc JSON từ stdin"]:::step
        H2["🔍 Parse: tool_name + file_path"]:::step
        H3{"🛡️ File path có '.env'?"}:::check
        H4["🚫 console.error + exit 2<br/>→ Chặn + báo Claude"]:::block
        H5["✅ exit 0 → Cho phép"]:::allow
        H1 --> H2 --> H3
        H3 -->|Có| H4
        H3 -->|Không| H5
    end

    P --> hook_script

    classDef pre fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:2px
    classDef post fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef step fill:#e0f2fe,stroke:#0284c7,stroke-width:1px,color:#0c4a6e
    classDef check fill:#fef3c7,stroke:#f59e0b,stroke-width:2px,color:#92400e
    classDef block fill:#ef4444,color:#fff,stroke:#dc2626,stroke-width:2px
    classDef allow fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px

    style config fill:#f8fafc,stroke:#64748b,stroke-width:2px
    style hook_script fill:#f0fdf4,stroke:#16a34a,stroke-width:2px
```

---

## 10. Hooks hữu ích

```mermaid
flowchart TD
    subgraph hook1["🔷 Hook 1: TypeScript Type Checker"]
        A1["✏️ Claude sửa function signature"]:::action --> B1["⚡ Post-tool hook kích hoạt"]:::hook
        B1 --> C1["🔍 Chạy: tsc --no-emit"]:::check
        C1 -->|Có lỗi type| D1["📤 Feed lỗi về Claude"]:::error
        D1 --> E1["✅ Claude tự sửa call sites"]:::success
        C1 -->|Không lỗi| F1["✅ Tiếp tục bình thường"]:::success
    end

    subgraph hook2["🔷 Hook 2: Duplicate Code Prevention"]
        A2["📄 Claude tạo file mới<br/>trong queries/"]:::action --> B2["⚡ Post-tool hook kích hoạt"]:::hook
        B2 --> C2["🤖 Launch Claude instance thứ 2<br/>qua TypeScript SDK"]:::check
        C2 --> D2["🔍 So sánh code mới vs code cũ"]:::check
        D2 -->|Duplicate| E2["🚫 Exit 2 + feedback<br/>→ Claude reuse code cũ"]:::error
        D2 -->|Unique| F2["✅ Exit 0 → Cho phép"]:::success
    end

    classDef action fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef hook fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:2px
    classDef check fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:2px
    classDef error fill:#ef4444,color:#fff,stroke:#dc2626,stroke-width:2px
    classDef success fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:2px

    style hook1 fill:#eff6ff,stroke:#3b82f6,stroke-width:2px
    style hook2 fill:#eff6ff,stroke:#3b82f6,stroke-width:2px
```

---

## 11. Claude Code SDK

```mermaid
flowchart LR
    subgraph sdk["📦 Claude Code SDK"]
        direction TB
        CLI["⌨️ CLI Interface<br/>claude --print"]
        TS["📘 TypeScript Library<br/>import Claude from sdk"]
        PY["🐍 Python Library<br/>from claude_code import sdk"]
    end

    subgraph perms["🔐 Permissions"]
        direction TB
        R["🔓 Read-only<br/>Mặc định<br/>Files, dirs, grep"]
        W["🔒 Write<br/>Phải bật thủ công<br/>options.allowTools: edit"]
    end

    subgraph usecases["✅ Best Use Cases"]
        direction TB
        U1["🛠️ Helper commands"]
        U2["📜 Scripts tự động"]
        U3["🪝 Hook scripts"]
        U4["🏗️ Tích hợp pipeline lớn"]
    end

    sdk --> perms
    sdk --> usecases

    classDef sdkNode fill:#4f46e5,color:#fff,stroke:#3730a3,stroke-width:2px
    classDef permNode fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:1px
    classDef useNode fill:#16a34a,color:#fff,stroke:#15803d,stroke-width:1px

    class CLI,TS,PY sdkNode
    class R,W permNode
    class U1,U2,U3,U4 useNode

    style sdk fill:#eef2ff,stroke:#4f46e5,stroke-width:2px
    style perms fill:#fef3c7,stroke:#f59e0b,stroke-width:2px
    style usecases fill:#f0fdf4,stroke:#16a34a,stroke-width:2px
```

---

## 📊 Tổng quan kiến trúc Claude Code

```mermaid
graph TB
    DEV([👤 Developer]):::user

    subgraph entry["🚪 Điểm truy cập"]
        direction LR
        CC["⌨️ Claude Code<br/>Terminal"]
        GH["🐙 GitHub Actions<br/>CI/CD"]
        SDK["📦 Claude Code SDK<br/>TypeScript / Python"]
    end

    DEV --> entry
    entry --> LM["🧠 Claude Language Model"]:::ai

    LM --> TOOLS

    subgraph TOOLS["🛠️ Tool System"]
        direction LR
        T1["📂 File Read/Write"]
        T2["⚡ Command Execution"]
        T3["🔌 MCP Servers<br/>vd: Playwright"]
        T4["🐙 GitHub Tools<br/>comments, commits, PRs"]
    end

    subgraph CTX["📄 Context Management"]
        direction LR
        CTX1["Claude.md files<br/>Project / Local / Machine"]
        CTX2["@ file mentions"]
        CTX3["/init codebase scan"]
    end

    subgraph HOOKS["🪝 Hooks"]
        direction LR
        H1["🔒 Pre-tool use<br/>Có thể chặn"]
        H2["🔄 Post-tool use<br/>Feedback loops"]
    end

    subgraph CMDS["⚡ Custom Commands"]
        CMD1[".claude/commands/*.md"]
    end

    CC --> CTX
    CC --> HOOKS
    CC --> CMDS
    CTX -.->|Cung cấp context| LM
    HOOKS -.->|Kiểm soát| TOOLS

    classDef user fill:#4f46e5,color:#fff,stroke:#3730a3,stroke-width:3px
    classDef ai fill:#7c3aed,color:#fff,stroke:#6d28d9,stroke-width:3px
    classDef entryNode fill:#0891b2,color:#fff,stroke:#0e7490,stroke-width:2px
    classDef toolNode fill:#f59e0b,color:#000,stroke:#d97706,stroke-width:1px
    classDef ctxNode fill:#60a5fa,color:#fff,stroke:#2563eb,stroke-width:1px
    classDef hookNode fill:#fb923c,color:#000,stroke:#ea580c,stroke-width:1px
    classDef cmdNode fill:#a78bfa,color:#fff,stroke:#7c3aed,stroke-width:1px

    class CC,GH,SDK entryNode
    class T1,T2,T3,T4 toolNode
    class CTX1,CTX2,CTX3 ctxNode
    class H1,H2 hookNode
    class CMD1 cmdNode

    style entry fill:#f0f9ff,stroke:#0891b2,stroke-width:2px
    style TOOLS fill:#fef3c7,stroke:#f59e0b,stroke-width:2px
    style CTX fill:#dbeafe,stroke:#3b82f6,stroke-width:2px
    style HOOKS fill:#ffedd5,stroke:#f97316,stroke-width:2px
    style CMDS fill:#ede9fe,stroke:#7c3aed,stroke-width:2px
```

---

## 📝 Bảng tóm tắt nhanh

| Tính năng | Shortcut / Lệnh | Mục đích |
|---|---|---|
| Memory Mode | `#` | Chỉnh sửa Claude.md thông minh |
| File mention | `@filename` | Thêm context cụ thể |
| Plan Mode | `Shift+Tab ×2` | Nghiên cứu & lên kế hoạch rộng |
| Thinking Mode | `"Ultra think"` | Lý luận sâu cho logic phức tạp |
| Paste screenshot | `Ctrl+V` | Mô tả UI element |
| Dừng Claude | `Escape` | Đổi hướng giữa chừng |
| Tua lại | `Escape ×2` | Nhảy về điểm trước trong hội thoại |
| Tóm tắt hội thoại | `/compact` | Giữ knowledge, dọn clutter |
| Xóa hội thoại | `/clear` | Bắt đầu task mới |
| Khởi tạo project | `/init` | Tạo Claude.md từ codebase |
| Cài MCP server | `claude mcp add` | Mở rộng khả năng Claude |
| Cài GitHub App | `/install GitHub app` | Tích hợp CI/CD |

---

*📅 Cập nhật: 2026-03-28*