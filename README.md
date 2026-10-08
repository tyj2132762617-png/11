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
    
