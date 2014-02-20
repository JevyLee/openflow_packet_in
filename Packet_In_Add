//include/openflow/openflow.h
struct ofp_packet_in {
    struct ofp_header header;
    uint32_t buffer_id;     /* ID assigned by datapath. */
    uint16_t total_len;     /* Full length of frame. */
    uint16_t in_port;       /* Port on which frame was received. */
    uint8_t reason;         /* Reason packet is being sent (one of OFPR_*) */
    uint8_t pad;
 //   uint8_t data[0];        
                              /* Ethernet frame, halfway through 32-bit word,
                               so the IP header is 32-bit aligned.  The
                               amount of data is inferred from the length
                               field in the header.  Because of padding,
                               offsetof(struct ofp_packet_in, data) ==
                               sizeof(struct ofp_packet_in) - 2. */
     char  sid[20];
     char  nid[16];
     uint32_t  pid;
     uint16_t  flag;
     uint32_t  data;
};
OFP_ASSERT(sizeof(struct ofp_packet_in) == 64);
	
//udatapath/datapath.h	
void
dp_output_control(struct datapath *dp, struct ofpbuf *buffer, int in_port,
                  size_t max_len, int reason)
{  
    struct ofp_packet_in *opi;
    size_t total_len;
    uint32_t buffer_id;

    buffer_id = save_buffer(buffer);
    total_len = buffer->size;
    if (buffer_id != UINT32_MAX && buffer->size > max_len) {
        buffer->size = max_len;
    }
    opi = ofpbuf_push_uninit(buffer, offsetof(struct ofp_packet_in, data));
    opi->header.version = OFP_VERSION;
    opi->header.type    = OFPT_PACKET_IN;
    opi->header.length  = htons(buffer->size);
    opi->header.xid     = htonl(0);
    opi->buffer_id      = htonl(buffer_id);
    opi->total_len      = htons(total_len);
    opi->in_port        = htons(in_port);
    opi->reason         = reason;
    opi->pad            = 0;
    memset(opi->sid,0,20);
    memset(opi->nid,0,16);
    opi->pid    =1;
    opi->flag   =1;
    opi->data   =1;  
    send_openflow_buffer(dp, buffer, NULL);
}
