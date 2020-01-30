package org.giiwa.vue.web;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.util.List;

import org.apache.commons.fileupload.FileItem;
import org.giiwa.core.base.IOUtil;
import org.giiwa.core.bean.X;
import org.giiwa.core.conf.Global;
import org.giiwa.core.dfile.DFile;
import org.giiwa.core.json.JSON;
import org.giiwa.framework.bean.Disk;
import org.giiwa.framework.web.Controller;
import org.giiwa.framework.web.Path;
import org.postgresql.util.Base64;

public class ueditor extends Controller {

	String type = "file/";

	@Path(path = "upload", login = true)
	public void upload() {

		if (X.isSame(type, "scrawl/")) {
			String content = this.getHtml("content");
			if (X.isEmpty(content)) {
				this.response(JSON.create().append(X.STATE, "FAIL").append("ERROR", "缺少参数，[content] "));
				return;
			}

			byte[] bytes1 = Base64.decode(content);
			ByteArrayInputStream in = new ByteArrayInputStream(bytes1);

			String name = lang.format(System.currentTimeMillis(), "mmssS.jpg");
			String filename = "/upload/" + type + lang.format(System.currentTimeMillis(), "yyyy/MM/dd/HH/") + name;

			DFile f1 = Disk.seek(filename);

			try {

				IOUtil.copy(in, f1.getOutputStream());
				String site = Global.getString("site.url", null);
				if (!X.isEmpty(site)) {
					filename = site + filename;
				}
				this.response(JSON.create().append(X.STATE, "SUCCESS").append("url", filename).append("title", name)
						.append("original", name));

			} catch (Exception e) {
				log.error(e.getMessage(), e);
				this.response(JSON.create().append(X.STATE, "FAIL").append("ERROR", e.getMessage()));
			} finally {
				X.close(in);
			}

		} else {
			FileItem file = this.getFile("upfile");
			if (file == null) {
				this.redirect("/ueditor/config.json");
//				this.response(JSON.create().append(X.STATE, "FAIL").append("ERROR", "缺少参数，[upfile] "));
				return;
			}

			String filename = "/upload/" + type + lang.format(System.currentTimeMillis(), "yyyy/MM/dd/HH/")
					+ file.getName();

			DFile f1 = Disk.seek(filename);
			try {

				IOUtil.copy(file.getInputStream(), f1.getOutputStream());

				String site = Global.getString("site.url", null);
				if (!X.isEmpty(site)) {
					filename = site + filename;
				}

				this.response(JSON.create().append(X.STATE, "SUCCESS").append("url", filename)
						.append("title", file.getName()).append("original", file.getName()));

			} catch (Exception e) {
				log.error(e.getMessage(), e);
				this.response(JSON.create().append(X.STATE, "FAIL").append("ERROR", e.getMessage()));
			}
		}

	}

	@Path(path = "upload/image", login = true)
	public void upload_image() {
		type = "image/";
		upload();
	}

	@Path(path = "upload/video", login = true)
	public void upload_video() {
		type = "video/";
		upload();
	}

	@Path(path = "upload/scrawl", login = true)
	public void upload_scrawl() {
		type = "scrawl/";
		upload();
	}

	@Path(path = "listimage", login = true)
	public void listimage() {
		int start = this.getInt("start");
		int size = this.getInt("size", 20);

		try {
			List<JSON> l1 = JSON.createList();
			int total = _scan(Disk.seek("/upload/image/"), start, size, l1);

			this.response(JSON.create().append(X.STATE, "SUCCESS").append("list", l1).append("start", start)
					.append("total", total));
		} catch (Exception e) {
			log.error(e.getMessage(), e);
			this.response(JSON.create().append(X.STATE, "FAIL").append("ERROR", e.getMessage()));
		}
	}

	private int _scan(DFile f, int start, int size, List<JSON> l1) throws IOException {
		int t = 0;
		if (f.isDirectory()) {
			DFile[] ff = f.listFiles();
			if (ff != null) {
				for (DFile f1 : ff) {
					if (f1.isFile()) {
						if (start <= 0 && l1.size() < size) {
							String filename = f1.getFilename();
							String site = Global.getString("site.url", null);
							if (!X.isEmpty(site)) {
								filename = site + filename;
							}
							l1.add(JSON.create().append("url", filename));
						}
						t++;
					} else {
						t += _scan(f1, start - t, size, l1);
					}
				}
			}
		}
		return t;
	}

}
