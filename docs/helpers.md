



oc debug -t node/ncpvnpvhub-hubworker-101.ncpvnpvhub.pnwlab.nsn-rdnet.net -- chroot /host bash -c 'systemctl status kubelet | grep -i "main PID" && systemctl is-active kubelet'


oc debug -t node/ncpvnpvhub-hubworker-101.ncpvnpvhub.pnwlab.nsn-rdnet.net -- chroot /host bash -c ' kill -9 $(pgrep -f kubelet)'


oc debug -t node/ncpvnpvhub-hubworker-101.ncpvnpvhub.pnwlab.nsn-rdnet.net -- chroot /host bash -c 'systemctl status kubelet | grep -i Active:'




oc debug -t node/ncpvnpvlab1-master-101.ncpvnpvlab1.pnwlab.nsn-rdnet.net -- chroot /host bash -c '/usr/local/bin/cluster-backup.sh /home/core/assets/backup'