methods: { load(blob) { let u = window.URL.createObjectURL(new Blob([blob]));
let a = document.createElement("a");
a.download = this.dn || new Date().getTime();
a.href = u;
a.style.display = "none";
document.body.appendChild(a);
a.click();
a.remove();
}, progress(e) { if (e.lengthComputable) { //进度信息是否可用 let x = (e.loaded / e.total).toFixed(2) * 100;
this.p = x this.$emit('progress', x) } }, downVideo() { let xhr = new XMLHttpRequest();
if (this.disabled) { return false } if (!this.url) { this.$message({ type: 'error', message: '资源不存在或已被删除！！！' }) return false } xhr.open("GET", this.url, true);
xhr.responseType = "blob";
// 返回类型blob xhr.onload = (e) => { if (xhr.readyState === 4 && xhr.status === 200) { this.load(xhr.response);
// 转换一个blob链接 } };
xhr.onreadystatechange = (e) => { if (e.currentTarget.status != 200) { this.$message({ type: 'error', message: '资源不存在或已被删除！！！' }) } } xhr.onprogress = this.progress;
xhr.send();
}