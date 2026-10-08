# 11from pathlib import Path
from datetime import datetime

def scan_files(root: Path, extensions: list[str] | None = None) -> list[dict]:
    exts = {e.lower().lstrip(".") for e in extensions} if extensions else None
    results = []
    for p in sorted(root.rglob("*")):
        if not p.is_file():
            continue
        if exts and p.suffix.lower().lstrip(".") not in exts:
            continue
        st = p.stat()
        results.append({
            "path": p,
            "name": p.name,
            "size": st.st_size,
            "mtime": datetime.fromtimestamp(st.st_mtime),
            "ext": p.suffix.lower(),
        })
    return results
    def cmd_scan(args):
    files = scan_files(Path(args.dir), args.ext)
    if not files:
        print("未找到匹配文件。")
        return
    print(f"{'文件名':<40} {'大小':>10}  {'修改时间':<20}")
    print("-" * 75)
    for f in files:
        print(f"{f['name']:<40} {f['size']/1024:>8.1f}KB  {f['mtime']:%Y-%m-%d %H:%M:%S}")
    print(f"\n共 {len(files)} 个文件。")
