---
layout: content
title:  "Sample Captures"
categories: tcpreplay wiki
description: "Tcpreplay �c�[���Q�Ŏg���T���v���L���v�`���t�@�C���^List of sample network capture files for use with Tcpreplay suite"
---

- [�T�v�^Overview][Overview](#overview)
    - [������ flow�^smallFlows.pcap][smallFlows.pcap](#smallflows-pcap)
    - [�傫�� flow�^bigFlows.pcap][bigFlows.pcap](#bigflows-pcap)
    - [�e�X�g�^test.pcap][test.pcap](#test-pcap)
- [Contributions][Contributions](#contributions "Project Contributions")


<h2 id="Sample Captures"><a name="overview"></a>�T�v�^Overview</h2>
Tcpreplay �c�[���Q�͂����Ă��̃L���v�`���t�@�C����ǂݍ��߂܂��B
�ȒP�Ɏ����Ă݂邽�߂ɁA�T���v���̃L���v�`���t�@�C����p�ӂ��܂����B
[IP Flow][flow]/[NetFlow][NetFlow] ���e�X�g���邽�߂ɏ������ꂽ���̂ł����A
�X�C�b�`��l�b�g���[�N�@��̃p�t�H�[�}���X����������̂ɂ��g���܂��B

pcap �t�@�C���� [������^here][pcaps] �ɂ���܂��B

### <a name="smallflows-pcap"></a>[������ flow�^smallFlows.pcap][small]
���̃L���v�`���t�@�C���́A
�������̈قȂ�A�v���P�[�V�����̃L���v�`���t�@�C��������܂����B
��r�I�l�b�g���[�N�g���t�B�b�N�����Ȃ����ɂ����āA
�l�X�ȃv���g�R���ł�������� flow �����悤�ɍl�����Ă��܂��B
�����ȃT�C�Y�̃t�@�C���ł�������� flow ���g�������ꍇ��A
flow �̑g�ݍ��킹�̍Č����ɔz�����Ȃ��ėǂ��ꍇ�Ȃǂ́A
���̃L���v�`���t�@�C�����g���Ă݂Ă��������B

Size:			9.4 MB   
Packets:		14261   
Flows:			1209   
Average packet size:	646 bytes   
Duration:		5 minutes   
Number Applications:	28


### <a name="bigflows-pcap"></a>[�傫�� flow�^bigFlows.pcap][big]
���̃L���v�`���t�@�C���́A
���G�����v���C�x�[�g�l�b�g���[�N�̃C���^�[�l�b�g�ւ̏o���ɂ�����A
���ۂ̃l�b�g���[�N�g���t�B�b�N���L���v�`���������̂ł��B
�O�̃L���v�`���t�@�C��([������ flow�^smallFlows.pcap][small])�Ɣ�ׂ�ƁA
�t�@�C���T�C�Y�͂��Ȃ�傫���A�p�P�b�g�̕��σT�C�Y�͏������ł��B
�قȂ�A�v���P�[�V�����̂�������� flow ���܂܂�Ă��܂��B
�傫�ȃT�C�Y�̃t�@�C������舵����̂ł���΁A
���̃t�@�C���Ńe�X�g���Ă��������B

Size:			368 MB   
Packets:		791615    
Flows:			40686  
Average packet size:	449 bytes   
Duration:		5 minutes   
Number Applications:	132

### <a name="test-pcap"></a>[�e�X�g�^test.pcap][test]
�\�[�X�R�[�h�� tar.gz �ɓ�������Ă��鏬���ȃL���v�`���t�@�C���ł��B
`sudo make test` �̎��� *tcpreplay* �̐��x���e�X�g����̂ɂ��g���܂��B
*tcpprep* �̐��\���f������̂ɂ��g���܂��B

Size:           0.07 MB   
Packets:        141    
Flows:          37  
Average packet size:    445 bytes   
Duration:       3 seconds   
Number Applications:    1


## <a name="contributions"></a>Contributions�^Contributions
Tcpreplay �œ������̂ɗǂ��T���v���� pcap �t�@�C��������Ȃ�A
���Ѓ����e�i�ɘA�����Ă��������B

[flow]:     https://ietf.org/wg/ipfix/
[NetFlow]:  http://www.cisco.com/go/netflow
[pcaps]:    https://www.dropbox.com/sh/j8i8v1nujvbumyb/ujd3biK6Fk/captures
[small]:    https://dl.dropboxusercontent.com/u/98306176/captures/smallFlows.pcap
[big]:      https://dl.dropboxusercontent.com/u/98306176/captures/bigFlows.pcap
[test]:     https://dl.dropboxusercontent.com/u/98306176/captures/test.pcap
