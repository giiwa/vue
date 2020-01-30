package org.giiwa.vue.web;

import org.apache.commons.configuration2.Configuration;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.giiwa.framework.bean.License;
import org.giiwa.framework.web.IListener;
import org.giiwa.framework.web.Module;

public class VueListener implements IListener {

	static Log log = LogFactory.getLog(VueListener.class);

	public void onInit(Configuration conf, Module m) {
		log.info("webvue is initing...");

		m.setLicense(License.LICENSE.licensed,
				"OYKGmDEbI+rZraetYN/k2WvUdIzkZf7Jz8QkZ7ZgSEScFD/0khFfPL9dxfCW4NjVDhrb/KxHoCt0Lb6zFP2proMhxz7+dwr2geosojT1HgMPd7pvd9LpYOju47Oc7HXXhtudJIVOLD6CjjHfTbgBzGjqiwrRjjeLBwDZw/vmpAE=");

	}

	@Override
	public void onStart(Configuration conf, Module m) {
		// TODO Auto-generated method stub
		log.info("webvue is starting ...");

	}

	@Override
	public void onStop() {
		// TODO Auto-generated method stub
		log.info("webvue is stopping ...");

	}

	@Override
	public void uninstall(Configuration conf, Module m) {
		// TODO Auto-generated method stub

	}

	@Override
	public void upgrade(Configuration conf, Module m) {
		// TODO Auto-generated method stub

	}

}
