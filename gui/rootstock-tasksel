#!/usr/bin/python
""" Tasksel UI that spits out a list of selected tasks """
import pygtk
pygtk.require("2.0")
import gtk, gobject

iconpath="../images/rootstock_icon.png"
retval = []

class GUI:
    def __init__(self):
        self.root = gtk.Window(type=gtk.WINDOW_TOPLEVEL)
        self.root.set_title("Select Tasks for installation")
        self.root.connect("destroy", self.destroy_cb)
        self.root.set_default_size(400, 300)
        self.root.set_icon_from_file(iconpath)
        self.scroll = gtk.ScrolledWindow(hadjustment=None, vadjustment=None)
        self.box = gtk.VBox()
        self.box2 = gtk.VBox()
        self.hbox = gtk.HBox()
        self.close = gtk.Button(label=None, stock=gtk.STOCK_APPLY, use_underline=False)
        self.close.connect('clicked', self.destroy_cb)

        self.viewport = gtk.Viewport()
        self.viewport.add(self.box)
        self.scroll.add(self.viewport)
        self.scroll.set_policy(gtk.POLICY_NEVER, gtk.POLICY_AUTOMATIC)

        self.box2.add(self.scroll)
        self.hbox.pack_end(self.close, expand=False, fill=False, padding=5)
        self.box2.pack_end(self.hbox, expand=False, fill=False, padding=5)
        self.root.add(self.box2)
        tasknames, taskdesc = self.read_tasklist()
        while len(taskdesc):
            descr = taskdesc.pop()+':  '+tasknames.pop()
            widget = gtk.CheckButton(descr)
            widget.connect('toggled', self.checkbox_toggled)
            self.box.pack_start(widget, False, False, 0)
        self.root.show_all()

    def destroy_cb(self, *kw):
        """ Destroy callback to shutdown the app """
        val = ""
        while len(retval):
            val = val+' ^'+retval.pop()
        print val.lstrip()
        gtk.main_quit()
        return

    def run(self):
        """ run is called to set off the GTK mainloop """
        gtk.main()
        return

    def read_tasklist(self):
        f = open('/usr/share/tasksel/ubuntu-tasks.desc', 'r')
        tasknames = []
        taskdesc = []
        for line in f:
            if line.startswith("Task:"):
                tasknames.append(line.split(":")[1].strip())
            elif line.startswith("Description:"):
                taskdesc.append(line.split(":")[1].strip())
        return tasknames, taskdesc

    def checkbox_toggled(self, widget):
        val = str(widget.get_label()).split(":")[1].strip()
        if widget.get_active():
            retval.append(val)
        else:
            retval.remove(val)

if __name__ == '__main__':
    myGUI = GUI()
    myGUI.run()

